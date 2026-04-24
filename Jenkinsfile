pipeline {
    agent any

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
                sh 'docker build -t api-app .'
            }
        }
    
    }   
}