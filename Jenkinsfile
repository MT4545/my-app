pipeline{
   agent any
   stages{
       stage("SCM checkout"){
           steps{
               git branch: 'dev', url: 'https://github.com/javahometech/my-app'
           }
       }
       
       stage("Maven Build"){
           steps{
               sh 'mvn clean package'
               sh 'mv target/myweb*.war target/myweb.war'
           }
       }
       
       stage("Deploy to Tomcat Development"){
           steps{
               sshagent(['dev-tomcat-1']) {
                  sh  "scp -o StrictHostKeyChecking=no target/myweb*.war ec2-user@172.31.33.177:/opt/tomcat8/webapps/"
                  sh  "ssh ec2-user@172.31.33.177 /opt/tomcat8/bin/shutdown.sh"
                  sh  "ssh ec2-user@172.31.33.177 /opt/tomcat8/bin/startup.sh"
                   
              }
           }
       }
   }
       post {
       always {
           cleanWs()
    }
  }
}


