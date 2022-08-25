pipeline {
  agent any
  
  stages {
    stage ("Maven Build") {
       steps {
         sh 'mvn clean package'
      }
    }
   stage ("Deploy To Dev") {
       steps {
         sshagent(['tomcat']) {
          // copy war file to tomcat
         sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.4.165:/opt/tomcat8/webapps/app.war'
         // stop tomcat
         sh 'ssh ec2-user@172.31.4.165 /opt/tomcat8/bin/shutdown.sh'
         // start tomcat
         sh 'ssh ec2-user@172.31.4.165 /opt/tomcat8/bin/startup.sh'
         }
      }
    }
  }
  post {
      success {
          archiveArtifacts artifacts: 'target/*.war'
          cleanWs()
      }
   }
}
