node
{
    
def mavenHome = tool name: "maven3.8.1"

properties([pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckoutCode')   
{
git branch: 'development', credentialsId: 'a3090c1a-558a-4a89-9e90-f0742273b9ed', url: 'https://github.com/sudharshandevops1991/maven-web-application.git'
}

stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}
 
stage('ExecuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactsintoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppintoTomcatServer')
{
sshagent(['TomcatServer']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.149.9:/opt/apache-tomcat-9.0.55/webapps/"
}
}

stage('SendEmailNotification')
{
emailext body: '''build over

Regards,
sudharshan.''', subject: 'build over', to: 'sudharshan.cloud1@gmail.com'   
}

}
