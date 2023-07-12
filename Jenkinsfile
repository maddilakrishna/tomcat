pipeline {
  agent any
  stages {
    stage("clear workspace"){
        steps {
          sh 'sudo rm -rvf t*'
        }
    }
    stage("Checkout") {
        steps {
        sh 'git clone https://github.com/KrishnaMaddila/tomcat.git'
      }
    }
    stage('GCP Auth') {
        steps {
        withCredentials([usernameColonPassword(credentialsId: '7b112cd2-2be5-470d-a959-8dfb349f1de5', variable: 'GCP_PROJECT'), file(credentialsId: '3e4bdfc2-80cd-473b-a505-090d40869d34', variable: 'GCP_CREDENTIALS')]) {
         sh 'gcloud auth activate-service-account --key-file=$GCP_CREDENTIALS'
        }
      }
    }
    stage("Docker Pull") {
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
    stage("docker push") { 
        steps {
             sh 'gcloud auth configure-docker'
             sh 'docker push gcr.io/model-argon-389809/tomcat:test1'
           }
        }
     stage("cluster create") {
       steps {
          sh 'gcloud components install kubectl'
          sh 'gcloud config set compute/zone asia-south1-b'
          sh 'gcloud container clusters create tomcat-cluster --num-nodes 3'
       }
     }
    stage("create & expose deploy") {
       steps {
         sh 'kubectl create deployment apachetomcat --image=gcr.io/	model-argon-389809/tomcat:test1'
         sh 'kubectl expose deployment apachetomcat --type=LoadBalancer --port 80 --target-port 8080'
       }
    }
  }  
}    
