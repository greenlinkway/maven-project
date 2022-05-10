pipeline {
  agent any
  triggers {
    pollSCM('*/5 * * * *')
  }
  stages{
       stage ('Build'){
        steps {
          sh 'mvn clean package'
        }
         post {
           success {
             echo 'Archiving...'
             archiveArtifacts artifacts:'**/target/*.war'
           }
         }
       }
       stage ('Deployments') {
         parallel{
           stage ('Deploy to Staging'){
             steps {
               sh "cp **/target/*.war /home/edu/tomcat_staging/webapps"
             }
           }
           stage ('Deploy to prod') {
             steps {
               sh "scp **/target/*.war root@10.137.0.32:/home/edu/tomcat_prodaction/webapps"
             }     
           }
         }
       }
    }
} 
  
