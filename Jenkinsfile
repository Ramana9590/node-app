pipeline {
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Image'){
            steps{
                sh "docker build . -t ramana3/node-app:${DOCKER_TAG} "
            }
        }
        stage('DockerHub Push'){
            steps{
                withCredentials([string(credentialsId: 'Docker_hub_passwd', variable: 'Docker_hub_passwd')]) {
                    sh "docker login -u ramana3 -p ${Docker_hub_passwd}"
                    sh "docker push ramana3/node-app:${DOCKER_TAG} "
                }
            }
        }
        stage('Deploy to k8s env'){
            steps{
                  sh "chmod +x changeTag.sh "
                  sh "./changeTag.sh ${DOCKER_TAG}"
                sshagent (credentials: ['kops-mechine']) {
                                  sh "scp -u StrictHostKeyChecking=no services.yml node-app-pod.yml admin@54.242.126.19:/home/admin/"
				    script{
					    try{ 
                                              sh "ssh admin@54.242.126.19 kubectl apply -f ."
                                          } catch(error){  
                                                       sh "ssh admin@54.242.126.19 kubectl create -f . "
                                                    }

					}
				}
            }
        }
    }
}

def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}
