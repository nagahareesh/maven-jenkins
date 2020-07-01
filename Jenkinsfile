node
{
    echo "GitHub BranhName ${env.BRANCH_NAME}"
      echo "Jenkins Job Number ${env.BUILD_NUMBER}"
      echo "Jenkins Node Name ${env.NODE_NAME}"
  
      echo "Jenkins Home ${env.JENKINS_HOME}"
      echo "Jenkins URL ${env.JENKINS_URL}"
      echo "JOB Name ${env.JOB_NAME}"
    properties([[$class: 'JiraProjectProperty'], buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '3')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
def MavenHome = tool name: "Maven3.6.3"
stage('checkoutcode') 
{
  git branch: 'master', credentialsId: '4099d9fe-436b-43db-8a3f-e9a13d0141b3', url: 'https://github.com/nagahareesh/maven-jenkins.git'  
}
stage('Build')
{
sh "${MavenHome}/bin/mvn clean package"
}
stage('codeQuality')
{
 sh "${MavenHome}/bin/mvn sonar:sonar"
}
stage('ArtifactoryRepositry')
{
 sh "${MavenHome}/bin/mvn deploy"
}
stage('deploytoTomcat')
{
    sshagent(['baed1ce1-fba9-4b99-bc8d-b8abe99a96aa'])
    {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.51.252:/opt/apache-tomcat-9.0.36/webapps/"
}
}
}

