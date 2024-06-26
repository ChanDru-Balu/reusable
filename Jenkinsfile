pipeline{
    agent any
       tools {
        // Define the Docker tool in the 'tools' section
        // This assumes 'myDockerTool' is configured in Jenkins
        dockerTool 'MyDockerTool'
    }
    environment {
        // Define environment variables if needed
        dockerImage = 'reusable-image'
    }

    stages{
        stage('Clone Repository'){
            steps{
                git 'https://github.com/ChanDru-Balu/reusable'
            }
        }

        stage('Build Angular Production') {
            steps {
                script {
                    // Navigate to Angular project directory
                    dir('angular-project') {
                        // Build Angular project for production
                        bat "npm install"
                        bat "npm run build --prod"
                    }
                }
            }
        }

        stage('Build Docker Image'){
            steps{
                script{
                //     sh '/usr/local/bin/docker build -t reusable-image .'

                   // Get the current working directory
                    def currentDir = pwd()

                    // Define the path to the Dockerfile relative to the Jenkinsfile
                    def dockerfilePath = "${currentDir}/Dockerfile"

                    // Build Docker image
                    bat "docker build -t reusable-image -f ${dockerfilePath} ."
                } 
            }
        }
    
        stage('Run Docker Container'){
            steps{
                script {
                    // sh '/usr/local/bin/docker run -p 8090:80 reusable-image .'

            // Run Docker container
            bat "docker run -p 8090:80 -d --name reusable-container reusable-image"
                }
            }
        }

    }
}
