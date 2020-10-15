pipeline {
    agent {
        docker {
            image 'maven-agent:2'
            registryCredentialsId 'nexus_deployer'
            registryUrl 'http://35.239.228.90:5000/'
        }
    }
    stages {
        stage('clone project') { 
            steps {
                git 'https://github.com/AMMiller/boxfuse.git'
            }
        }
        stage('Build') { 
            steps {
                sh 'pwd'
            }
        }
    }
}