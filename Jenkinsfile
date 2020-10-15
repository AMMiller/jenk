pipeline {
    agent none
    stages {
        stage('clone project') { 
            agent {
                docker {
                    image 'maven-agent:2'
                    registryCredentialsId 'nexus_deployer'
                    registryUrl 'http://35.239.228.90:5000/'
                }
            }
            steps {
                git 'https://github.com/AMMiller/boxfuse.git'
            }
        }
        stage('Build') { 
            agent any
            steps {
                git 'https://github.com/AMMiller/docker-tomcat8.git'
                sh 'cp -R ./target/hello-1.0  ./tomcat8-boxfuse'
                sh 'cd tomcat8-boxfuse && docker build -t tomcat8-boxfuse .'
                sh 'pwd'
            }
        }
    }
}