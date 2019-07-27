node()
{
  echo "GitHub BranhName ${env.BRANCH_NAME}"
  echo "Jenkins Job Number ${env.BUILD_NUMBER}"
  echo "Jenkins Node Name ${env.NODE_NAME}"
  
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"
 
    def mvnHome = tool name: 'maven 3.6.1', type: 'maven'
 properties([
       buildDiscarder(logRotator(numToKeepStr: '3')),
       /*pipelineTriggers([
           pollSCM('* * * * *')
       ])*/
   ])
 
    stage('Checkout')
    {
       git branch: 'Development', credentialsId: 'Github', url: 'https://github.com/dsktechnologies/maven-web-application.git'
       
    }
    stage('Build')
    {
        sh "${mvnHome}/bin/mvn clean package"
    }
    stage('ExecuteSonarqubereport')
    {
        sh "${mvnHome}/bin/mvn sonar:sonar"
    }
    stage('Uploadartifacts')
    {
        sh "${mvnHome}/bin/mvn deploy"
    }
    stage('DeployApplication')
    {
        sshagent(['b64603ce-a6df-4a46-b79a-c3bc3a726f21']) 
        {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@35.178.85.180:/opt/apache-tomcat-9.0.21/webapps"
        }
     }
    
}
