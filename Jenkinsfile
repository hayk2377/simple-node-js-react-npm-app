pipeline {
    agent any
    stages {

        stage('Test') {
            steps {
                // Run tests using Docker Compose
                bat 'docker-compose run --rm app npm test -- --watchAll=false'
            }
        }
        stage('Cleanup') {
            steps {
                // Clean up containers, volumes, and networks
                bat 'docker-compose down --volumes --remove-orphans'
            }
        }
        stage('Deploy') {
            steps {
                // Deploy the application
                bat 'docker-compose up -d'
                // Wait for user input before proceeding
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                // Stop the application after confirmation
                bat 'docker-compose down'
            }
        }
    }
}