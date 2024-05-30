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
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building the project...'
                    bat 'mvn clean install'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    bat 'mvn test'
                }
            }
        }

        stage('Code Quality Analysis') {
            steps {
                script {
                    echo 'Running code quality analysis...'
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
                    withCredentials([usernamePassword(credentialsId: 'My-Docker-Id', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        bat 'docker login -u %DOCKER_USERNAME% -p %DOCKER_PASSWORD%'
                        bat 'docker-compose down'
                        bat 'docker-compose up -d'
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
                }
            }
        }

        stage('Monitoring and Alerting') {
            steps {
                script {
                    echo 'Setting up monitoring and alerting...'
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
