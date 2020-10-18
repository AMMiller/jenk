pipeline {
    agent none
    environment { 
        // registry data
        registry = '34.122.111.150:5000' 
        registryCredential = 'nexus_deployer' 
        imageName = "tomcat8-image"
        dockerImage = '' 

    }
    stages {
        stage('clone project') { 
            agent {
                docker {
                    image 'maven-agent:latest'
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
    }
}