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

         stage('Deploy to GitHub Pages') {
            steps {
                script {
                       // Check if 'gh-pages' directory exists, if not create it
                    if (!fileExists('gh-pages')) {
                        bat "mkdir gh-pages"
                    }

                    // Copy website files from Docker container to 'gh-pages' directory
                    bat "docker cp reusable-container:/usr/share/nginx/html ./gh-pages"

                    // Navigate to the 'gh-pages' directory
                    dir('gh-pages') {
                        // Initialize Git repository
                        bat "git init"
                        
                        // Add all files
                        bat "git add ."

                        // Configure Git user name and email
                        bat "git config --global user.email 'prochandru@gmail.com'"
                        bat "git config --global user.name 'ChanDru-Balu'"

                        
                        // Commit changes
                        bat 'git commit -m "Deploy to GitHub Pages"'
                        
                        // // Add remote repository
                        bat "git remote add origin https://github.com/ChanDru-Balu/reusable"
                        
                        // // Push to the 'gh-pages' branch
                        // bat "git push -u origin master --force"

                        // Create and switch to 'gh-pages' branch
                        bat "git checkout -b gh-pages"
                        
                        // Push the 'gh-pages' branch to remote repository
                        bat "git push -u origin gh-pages"

                    }
                }
            }
        }
    }
}