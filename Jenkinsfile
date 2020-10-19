pipeline {
    agent none
    environment { 
        // registry data
        registry = '35.226.97.39:5000' 
        registryCredential = 'nexus_deployer' 
        imageName = "tomcat8-image"
        dockerImage = '' 
        stageServer = 'tcp://35.228.194.187:2375'
    }
    stages {
        stage('clone project') { 
            agent {
                docker {
                    image 'maven-agent'
                    registryCredentialsId "$registryCredential"
                    registryUrl "$registry"
                }
            }
            steps {
                git 'https://github.com/AMMiller/boxfuse.git'
            }
        }
        stage('Get dockerfile') { 
            agent any
            steps {
                git 'https://github.com/AMMiller/docker-tomcat8.git'
            }
        }
        stage('Build') { 
            agent any
            steps {
                script { 
                    dockerImage = docker.build registry + '/$imageName'
                }
            }
        }
        stage('Deploy docker image') { 
            steps { 
                script { 
                    docker.withRegistry( 'http://' + registry, registryCredential ) { 
                        dockerImage.push() 
                    }
                } 
            }
        } 
        stage('Cleaning up') { 
            steps { 
                sh 'docker rmi $registry/$imageName'
            }
        } 

        stage('Run docker tomcat8 container on stage') { 
            steps { 
                script {
                    docker.withServer(stageServer) {
                        docker.withRegistry('http://' + registry, registryCredential) {
                            docker.image('$registry/$imageName').run('-p 80:8080 --name $imageName')
                        }
                    }
                }
            }
        }

    }
}