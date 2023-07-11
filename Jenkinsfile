pipeline {
  agent any
  stages {
    stage("Checkout") {
        steps {
        sh 'git clone https://github.com/Poojitha2022/nodeapp_test.git'
      }
    }
    stage('GCP Auth') {
        steps {
        withCredentials([usernameColonPassword(credentialsId: '7b112cd2-2be5-470d-a959-8dfb349f1de5', variable: 'GCP_PROJECT'), file(credentialsId: '3e4bdfc2-80cd-473b-a505-090d40869d34', variable: 'GCP_CREDENTIALS')]) {
         sh 'gcloud auth activate-service-account --key-file=$GCP_CREDENTIALS'
         sh 'gcloud config set project $GCP_PROJECT'
        }
      }
    }
    stage("Docker Pull") {
      steps {
        sh 'docker pull krishnavirat/apachetomcat:tag2'
      }
    }
    stage("Docker tag") {
      steps {     
         sh 'docker tag krishnavirat/apachetomcat:tag2 gcr.io/model-argon-389809/tomcat:test1'
      }
    }
    stage("docker push") { 
        steps {        
             sh 'docker push gcr.io/model-argon-389809/tomcat:test1'
           }
        }
     }
  }    
