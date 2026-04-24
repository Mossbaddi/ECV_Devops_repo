pipeline {
    agent any

    options {
        disableConcurrentBuilds() // interdit le fait de lancer ce job 2 fois en meme temps
        parallelsAlwaysFailFast() // dans un parallel, si l'un des threads échoue, stoppe les autres
    }

    environment {
        IMAGE_NAME = "task-api"
    }

    stages {
        stage("Install") {
            steps {
                sh 'npm ci'
            }
        }
        // LANCEZ LES TESTS ET LE LINT

        stage("Qualité") {
            parallel {

                stage("Lint") {
                    steps {
                        sh "npm run lint "
                    }

                }
                stage("Tests") {
                    steps {
                        sh "npm run test:coverage"
                    }
                }
            }
        }

        // Faites un build docker de cette image en utilisant le container engine de l'hôte
        stage("Build") {
            steps {
                sh '''
                docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest
                '''
            }
        }


        stage("Deploy") {
            steps {
                sh 'docker run -d -p 3002:3000 ${IMAGE_NAME}:${BUILD_NUMBER}'
            }
        }
    
    }   
}