pipeline {
    agent any

    tools {
        maven 'Maven Integration with Jenkins'
        jdk 'JDK'
    }

    environment {
        SONARQUBE_SERVER = 'SonarQube'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from the SCM (Source Code Management)
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building the project...'
                    // Run Maven clean install to build the project
                    bat 'mvn clean install'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    // Run Maven test to execute the tests
                    bat 'mvn test'
                }
            }
        }

        stage('Wait for SonarQube') {
            steps {
                script {
                    echo 'Waiting for SonarQube to be ready...'
                    // Wait for a certain period to ensure SonarQube is up
                    sleep time: 30, unit: 'SECONDS' // Adjust the time as needed
                }
            }
        }

        stage('Code Quality Analysis') {
            steps {
                script {
                    echo 'Running code quality analysis...'
                    // Run SonarQube analysis
                    withSonarQubeEnv('SonarQube') {
                        bat 'mvn sonar:sonar'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying to test environment...'
                    // Use Docker credentials to login and deploy using docker-compose
                    withCredentials([usernamePassword(credentialsId: 'My-Docker-Id', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        bat 'docker login -u %DOCKER_USERNAME% -p %DOCKER_PASSWORD%'
                        bat 'docker-compose down' // Stop and remove existing containers
                        bat 'docker-compose up -d' // Start containers in detached mode
                    }
                }
            }
        }

        stage('Generate Turnitin Proof') {
            steps {
                script {
                    echo 'Generating proof for Turnitin submission...'
                    // Example command to generate a text file with pipeline details
                    bat 'echo Pipeline completed successfully! > proof.txt'
                    // If you have Pandoc installed, you can generate a PDF
                    // bat 'pandoc -o proof.pdf proof.txt'
                }
            }
        }

        stage('Release') {
            steps {
                script {
                    echo 'Releasing to production...'
                    // Add steps for releasing to production if needed
                }
            }
        }

        stage('Monitoring and Alerting') {
            steps {
                script {
                    echo 'Setting up monitoring and alerting...'
                    // Add steps for setting up monitoring and alerting if needed
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
