pipeline{
    agent any
    tools {
        dockerTool 'ReusableDocker'
    }
    stages{
        stage('Clone Repository'){
            steps{
                git 'https://github.com/ChanDru-Balu/reusable'
            }
        }
        stage('Build Docker Image'){
            steps{
                script{
                    sh '/usr/local/bin/docker build -t reusable-image .'
                } 
            }
        }
    
        stage('Run Docker Container'){
            steps{
                script {
                    sh '/usr/local/bin/docker run -p 8090:80 reusable-image .'
                }
            }
        }
    }
}