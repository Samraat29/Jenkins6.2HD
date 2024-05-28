pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK'
    }

    environment {
        SCANNER_HOME = tool 'SonarQubeScanner'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Samraat29/Jenkins6.2HD.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    // Assuming pom.xml is in the root of the repository
                    dir('') {
                        bat 'mvn clean install'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    dir('') {
                        bat 'mvn test'
                    }
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    script {
                        dir('') {
                            bat "${SCANNER_HOME}/bin/sonar-scanner"
                        }
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
