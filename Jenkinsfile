node {
    def mavenHome = tool name: 'maven'

    stage('git checkout')
    {
      git branch: 'dev', url: 'https://github.com/HemanthVarupula/maven-webapplication.git'
    }
    stage('Build') {
    sh "${mavenHome}/bin/mvn clean package"
}
stage('SonarQube  Report') {
   sh "${mavenHome}/bin/mvn sonar:sonar"
}
stage('Upload to Nexus') {
    sh "${mavenHome}/bin/mvn deploy"
}
stage('Deploy to Tomcat') {
    echo "Deploying WAR file using curl..."

    sh """
        curl -u hemanth:password \
        --upload-file /var/lib/jenkins/workspace/First_MBPL/target/maven-web-application.war \
        "http://54.243.25.7:8080/manager/text/deploy?path=/maven-web-application&update=true"
    """
}

}
