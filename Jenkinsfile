pipeline {
  agent any
  stages {
    stage("Clear Workspace"){
        steps {
          sh 'rm -rvf t*'
        }
    }
    stage("Checkout") {
        steps {
        sh 'git clone https://github.com/KrishnaMaddila/tomcat.git'
      }
    }
    stage('GCP Auth') {
        steps {
         withCredentials([usernameColonPassword(credentialsId: 'da26742f-960a-47c0-b4c7-ea5de5f08ee6', variable: 'viratkrishna70@gmail.com'), file(credentialsId: '91c28a49-8eb4-44dd-bab8-249d93fea94b', variable: 'model-argon-389809-61e9c55645ae.json')]) {
         sh 'gcloud auth activate-service-account --key-file=$GCP_CREDENTIALS'
        }
      }
    }
    stage("Docker pull") {
      steps {
        withCredentials([usernameColonPassword(credentialsId: 'dockerHub', variable: 'dockerHub')]) {
        sh "docker login -u krishnavirat -p Krishna@2000"
        sh 'docker pull krishnavirat/apachetomcat:tag2'
       }
     }
    }
    stage("Docker tag") {
      steps {     
         sh 'docker tag krishnavirat/apachetomcat:tag2 gcr.io/model-argon-389809/tomcat:test1'
      }
    }
    stage("Docker push") { 
        steps {
             sh 'gcloud auth configure-docker'
             sh 'docker push gcr.io/model-argon-389809/tomcat:test1'
           }
        }
     stage("cluster create") {
       steps {
          sh 'gcloud container clusters create tomcat-cluster --num-nodes 3 --location=asia-south1-b'
       }
     }
    stage("Create & expose deploy") {
       steps {
         sh 'kubectl create deployment apachetomcat --image=gcr.io/model-argon-389809/tomcat:test1'
         sh 'kubectl expose deployment apachetomcat --type=LoadBalancer --port 80 --target-port 8080'
       }
    }
  }  
}    
