pipeline {
    agent any
    tools {
        dockerTool 'MyDockerTool'
    }
    environment {
        dockerImage = 'reusable-image'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/ChanDru-Balu/reusable'
            }
        }

        stage('Build Angular Production') {
            steps {
                script {
                    dir('angular-project') {
                        bat "npm install"
                        bat "npm run build --prod"
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def currentDir = pwd()
                    def dockerfilePath = "${currentDir}/Dockerfile"
                    echo "Current Directory: ${currentDir}"
                    echo "Dockerfile Path: ${dockerfilePath}"
                    bat "docker build -t reusable-image -f ${dockerfilePath} ."
                }
            }
        }
    
        stage('Run Docker Container') {
            steps {
                script {
                    bat "docker run -p 8090:80 -d --name reusable-container reusable-image"
                }
            }
        }

        stage('Deploy to GitHub Pages') {
            steps {
                script {
                    if (!fileExists('gh-pages')) {
                        bat "mkdir gh-pages"
                    }

                    bat "docker cp reusable-container:/usr/share/nginx/html ./gh-pages"

                    dir('gh-pages') {
                        bat "git rev-parse --verify gh-pages || git checkout -b gh-pages"
                        bat "git add ."
                        echo "Git Status:"
                        bat "git status --porcelain"

                        def gitStatus = bat(script: 'git status --porcelain', returnStdout: true).trim()
                        echo "Git Status:"
                        echo gitStatus

                        if (gitStatus) {
                            bat 'git commit -m "Deploy to GitHub Pages2"'
                            bat "git config --global user.email 'prochandru@gmail.com'"
                            bat "git config --global user.name 'ChanDru-Balu'"
                            bat "git push -u origin gh-pages"
                        } else {
                            echo "No changes to commit."
                        }
                    }
                }
            }
        }
    }
}
