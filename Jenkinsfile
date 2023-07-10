pipeline {
  agent any
  environment {
    GCP_PROJECT = credentials('GCP_PROJECT')
  }
  stages {
    stage('Example') {
      steps {
        withCredentials([string(credentialsId: 'GCP_CREDENTIALS', variable: 'GCP_CREDENTIALS')]) {
          // Use GCP credentials within this block
          sh """
            gcloud auth activate-service-account --key-file=$GCP_CREDENTIALS
            gcloud config set project $GCP_PROJECT
            # Other GCP commands or operations
          """
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
}    
