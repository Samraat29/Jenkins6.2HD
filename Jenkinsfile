pipeline {
    agent any

    tools {
        maven 'Maven' // Use the Maven tool configured in Jenkins
    }

    environment {
        SONARQUBE_SERVER = 'SonarQube' // Name of the SonarQube server configured in Jenkins
    }

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the project...'
                    sh 'mvn clean install' // Adjust according to your project's build command
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    sh 'mvn test' // Adjust according to your project's test command
                }
            }
        }

        stage('Code Quality Analysis') {
            steps {
                script {
                    echo 'Running code quality analysis...'
                    withSonarQubeEnv('SonarQube') {
                        sh 'mvn sonar:sonar' // Adjust according to your project's SonarQube command
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying to test environment...'
                    // Example: Deploying using Docker Compose
                    sh 'docker-compose up -d'
                }
            }
        }

        stage('Release') {
            steps {
                script {
                    echo 'Releasing to production...'
                    // Example: Using AWS CodeDeploy
                    // sh 'aws deploy create-deployment --application-name MyApp --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name MyDeploymentGroup --description "My deployment" --github-location repository=Samraat29/Jenkins6.2HD,commitId=main'
                }
            }
        }

        stage('Monitoring and Alerting') {
            steps {
                script {
                    echo 'Setting up monitoring and alerting...'
                    // Example: Integrating with Datadog
                    sh 'datadog-agent start'
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
