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

        stage('Wait for SonarQube') {
            steps {
                script {
                    echo 'Waiting for SonarQube to be ready...'
                    sleep 30
                }
            }
        }

        stage('Code Quality Analysis') {
            steps {
                script {
                    echo 'Running code quality analysis...'
                    withSonarQubeEnv('SonarQube') {
                        withCredentials([string(credentialsId: 'SonarQube-Integration', variable: 'SONAR_TOKEN')]) {
                            bat 'mvn sonar:sonar -Dsonar.login=%SONAR_TOKEN%'
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
