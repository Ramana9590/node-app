node{
    stage("git checkout"){
        
      git 'https://github.com/Ramana9590/node-app.git' 
        
        
    }
    
    stage("Build Docker Image"){
        sh "docker build -t ramana3/node-app ."
    }
    
    stage("Docker image push to Docker Hub"){
        withCredentials([string(credentialsId: 'Docker_hub_passwd', variable: 'Docker_hub_passwd')]) {
       sh "docker login -u ramana3 -p ${Docker_hub_passwd}"
}
        sh "docker push ramana3/node-app "
    }
    
    
    }
}
