pipeline {
    agent any

    tools {
        // Tools required for the pipeline
        maven 'Maven Integration with Jenkins'
        jdk 'JDK'
    }

    environment {
        // SonarQube server environment variable
        SONARQUBE_SERVER = 'SonarQube'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from the repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Building the project using Maven
                    echo 'Building the project...'
                    bat 'mvn clean install'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Running the tests using Maven
                    echo 'Running tests...'
                    bat 'mvn test'
                }
            }
        }

        stage('Wait for SonarQube') {
            steps {
                script {
                    // Waiting for SonarQube to be ready
                    echo 'Waiting for SonarQube to be ready...'
                    sleep 30
                }
            }
        }

        stage('Code Quality Analysis') {
            steps {
                script {
                    // Running code quality analysis using SonarQube
                    echo 'Running code quality analysis...'
                    withSonarQubeEnv('SonarQube') {
                        withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                            bat "mvn sonar:sonar -Dsonar.login=${SONAR_TOKEN}"
                        }
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
                        bat 'docker-compose up -d'
                    }
                }
            }
        }

        stage('Release') {
            steps {
                script {
                    echo 'Releasing to production...'
                    // Additional release steps can be added here
                }
            }
        }

        stage('Monitoring and Alerting') {
            steps {
                script {
                    echo 'Setting up monitoring and alerting...'
                    // Additional monitoring and alerting steps can be added here
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
