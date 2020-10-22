pipeline {    
    agent any
    environment { 
        // registry data
        registry = '104.197.26.253:5000' 
        registryCredential = 'nexus_deployer' 
        imageName = "tomcat8-boxfuse"
        dockerImage = '' 
        stageServer = 'tcp://35.228.101.239:2375'
    }
    stages {
        // clone and build buxfuse
        stage('clone project') { 
            steps {
                script {
                    docker.withRegistry('http://' + registry, registryCredential) {
                        docker.image('$registry/maven-agent').run('-v /var/run/docker.sock:/var/run/docker.sock --rm --name maven-agent')
                    }
                }
            }
        }
        stage('Deploy docker image to registry') { 
            steps { 
                sh 'docker tag $imageName $registry/$imageName'
                script {
                    withDockerRegistry([url: 'http://' + registry, credentialsId: registryCredential]) {
                        sh 'docker push $registry/$imageName'
                    }
                }
            }
        } 
        // run tomcat8/boxfuse contaner on the stage server
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