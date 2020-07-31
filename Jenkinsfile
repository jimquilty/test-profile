pipeline {
  agent any 
  environment {
    HAB_ORIGIN = 'jimquilty-homelab'
    HAB_LICENSE = 'accept'
    HAB_CHANNEL = 'stable'
  }
stages {
   stage('Build Habitat Artifact') {
     steps {
       withCredentials([string(credentialsId: 'HAB-AUTH-TOKEN', variable: 'HAB_AUTH_TOKEN')]) {
         script {
           sh 'hab origin key download $HAB_ORIGIN --auth $HAB_AUTH_TOKEN --secret'
           sh 'hab origin key download $HAB_ORIGIN'
         }
       habitat task: 'build', directory: '.', origin: "${env.HAB_ORIGIN}", docker: true
       }
     }
   }
   stage('Upload Artifact to Builder') {
     steps {
       withCredentials([string(credentialsId: 'HAB-AUTH-TOKEN', variable: 'HAB_AUTH_TOKEN')]) {
         script {
           sh 'hab origin key download $HAB_ORIGIN --auth $HAB_AUTH_TOKEN --secret'
           sh 'hab origin key download $HAB_ORIGIN'
         }
         habitat task: 'upload', authToken: env.HAB_AUTH_TOKEN, lastBuildFile: "${workspace}/results/last_build.env"
       }
     }
   }
   stage('Promote Artifact to Stable') {
     steps {
      script {
          env.HAB_PKG = sh (
            script: "cat ${workspace}/results/last_build.env | grep pkg_ident | sed 's/.*=//'",
            returnStdout: true
            ).trim()
        }
        withCredentials([string(credentialsId: 'HAB-AUTH-TOKEN', variable: 'HAB_AUTH_TOKEN')]) {
          habitat task: 'promote', channel: "${HAB_CHANNEL}", artifact: "${env.HAB_PKG}", authToken: "${env.HAB_AUTH_TOKEN}"
        }
     }
   }
  }
}