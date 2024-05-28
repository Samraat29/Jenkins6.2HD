pipeline {
    agent any

    tools {
        maven 'Maven Integration with Jenkins' // Ensure Maven tool is correctly configured
        jdk 'JDK' // Ensure JDK tool is correctly configured
    }

    environment {
        SONARQUBE_SERVER = 'SonarQube' // Ensure SonarQube server is correctly configured
    }

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the project...'
                    bat 'mvn clean install' // Build the project
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    bat 'mvn test' // Run tests
                }
            }
        }

        stage('Code Quality Analysis') {
            steps {
                script {
                    echo 'Running code quality analysis...'
                    withSonarQubeEnv('SonarQube') {
                        bat 'mvn sonar:sonar' // Run code quality analysis
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
                        bat 'docker-compose up -d' // Deploy the application
                    }
                }
            }
        }

        stage('Release') {
            steps {
                script {
                    echo 'Releasing to production...'
                    // Uncomment and modify the following line as needed
                    // bat 'aws deploy create-deployment --application-name MyApp --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name MyDeploymentGroup --description "My deployment" --github-location repository=YourUsername/YourRepository,commitId=main'
                }
            }
        }

        stage('Monitoring and Alerting') {
            steps {
                script {
                    echo 'Setting up monitoring and alerting...'
                    // Uncomment and modify the following line as needed
                    // bat 'datadog-agent start'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!' // Message on success
        }
        failure {
            echo 'Pipeline failed!' // Message on failure
        }
    }
}
