pipeline {
    agent any
  
   stage('Build Docker Image'){
        sh 'docker build -t ramana3/node-app .'
    }
    
    stage('Push Docker Image'){
              withCredentials([usernamePassword(credentialsId: 'Docker_hub', passwordVariable: 'Docker_hub_pass', usernameVariable: 'Docker_hub')]) {
              sh "docker login -u ramana3 -p ${Docker_hub_pass}"
                }
      }
}
