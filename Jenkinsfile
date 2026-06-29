pipeline
{
  agent any
  
  tools
  {
    maven 'Maven 3.9.16'
  }
  
  triggers
  {
    pollSCM('* * * * *')
  }
  
  options
  {
    timestamps()
    buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))
  }
  
  stages
  {
    stage('Checkout Code from GitHub')
    {
      steps()
      {
       git branch: 'development', credentialsId: '454deace-f7ec-4f04-ac70-3b2795481fd2', url: 'https://github.com/vickey5450/Maven-Web-Application.git'
      }
    }
    
    stage('Build Project')
    {
      steps()
      {
        sh "mvn clean package"
      }
    }
    
    stage('Execute SonarQube Report')
    {
      steps()
      {
        sh "mvn clean sonar:sonar"
      }
    }
    
    stage('Upload Artifacts to Sonatype Nexus')
    {
      steps()
      {
        sh "mvn clean deploy"
      }
    }
    
    stage('Deploy Application to Tomcat')
    {
      steps()
       {
    sshagent(credentials: ['7749a94e-4363-45de-a3f4-6c3ed92aaa0f'])
        {
      sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.7.238.125:/opt/apache-tomcat-9.0.118/webapps"
          }
       }
    }
  }
}
