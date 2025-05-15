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
                sh 'docker-compose run --rm app npm test -- --watchAll=false'
            }
        }

        stage('Cleanup') {
            steps {
                sh 'docker-compose down --volumes --remove-orphans'
            }
        }
    }

    post {
        success {
            githubNotify(credentialsId: 'github-token', commitSha: "${env.GIT_COMMIT}", status: 'SUCCESS')
        }
        failure {
            githubNotify(credentialsId: 'github-token', commitSha: "${env.GIT_COMMIT}", status: 'FAILURE')
        }
        unstable {
            githubNotify(credentialsId: 'github-token', commitSha: "${env.GIT_COMMIT}", status: 'UNSTABLE')
        }
        aborted {
            githubNotify(credentialsId: 'github-token', commitSha: "${env.GIT_COMMIT}", status: 'ABORTED')
        }
    }
}
