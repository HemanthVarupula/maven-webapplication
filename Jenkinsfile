pipeline{
    agent any 
    tools{
        maven "maven"
    }
    stages{
        stage("git checkout"){
            steps{
                git branch:"master" ,url:"https://github.com/HemanthVarupula/maven-webapplication.git"
            }
        }
        stage("build"){
           steps{
              sh "mvn clean package"
           }
        }
        stage("sonar report"){
           steps{
               sh "mvn sonar:sonar"
           }
        }
        stage("nexus push"){
            steps{
                sh "mvn deploy"
            }
        }
        stage("tomcat deploy"){
            steps{
                echo "Deploying WAR file to Tomcat..."

                sh """
                    curl -u hemanth:password \
                    --upload-file target/maven-web-application.war \
                    "http://18.191.48.219:8080/manager/text/deploy?path=/maven-web-application&update=true"
                """

            }
        }
    }
    post {
  success {
    notifyBuild(currentBuild.result)
  }
  failure {
notifyBuild(currentBuild.result)
      }
}

}

def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)
}
