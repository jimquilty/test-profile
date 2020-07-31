pipeline {
  agent any 
  environment {
    HAB_ORIGIN = 'jimquilty-homelab'
  }
  stages {
   stage('Build Habitat Artifact') {
     steps {
       withCredentials([string(credentialsId: 'HAB-AUTH-TOKEN', variable: 'HAB_AUTH_TOKEN')]) {
         script {
           sh 'hab origin key download --secret $HAB_ORIGIN -z $HAB_AUTH_TOKEN'
           sh 'hab origin key download $HAB_ORIGIN'
         }
      //  sh 'hab studio build'
       habitat task: 'build', directory: '.', origin: "${env.HAB_ORIGIN}", docker: true
       }
     }
   }
   stage('upload') {
     steps {
       withCredentials([string(credentialsId: 'HAB-AUTH-TOKEN', variable: 'HAB_AUTH_TOKEN')]) {
         script {
           sh 'hab origin key download --secret $HAB_ORIGIN -z $HAB_AUTH_TOKEN'
           sh 'hab origin key download $HAB_ORIGIN'
         }
         habitat task: 'upload', authToken: env.HAB_AUTH_TOKEN, lastBuildFile: "${workspace}/results/last_build.env"
       }
     }
   }
  }
}