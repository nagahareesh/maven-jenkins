Skip to content
nagahareesh
/
maven-jenkins
1
0
0
Code
Issues
Pull requests
1
Actions
Projects
Wiki
Security
1
Insights
More
maven-jenkins/Jenkinsfile
@nagahareesh
nagahareesh Update Jenkinsfile
Latest commit 0594182 21 minutes ago
 History
 2 contributors
@devopstrainingblr@nagahareesh
We found a potential security vulnerability in one of your dependencies.
Only the owner of this repository can see this message.

35 lines (34 sloc)  1.15 KB
  
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

