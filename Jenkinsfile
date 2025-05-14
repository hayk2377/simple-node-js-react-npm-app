pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                sh 'docker compose run --rm app npm test -- --watchAll=false'
            }
        }

        stage('Cleanup') {
            steps {
                sh 'docker compose down --volumes --remove-orphans'
            }
        }

        // stage('Deploy') {
        //     steps {
        //         sh 'docker compose up -d'
        //         input message: 'Finished using the web site? (Click "Proceed" to continue)'
        //         sh 'docker compose down'
        //     }
        }
    }
}
