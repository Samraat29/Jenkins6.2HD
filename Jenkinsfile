pipeline {
    agent any

    tools {
        // Use the Maven tool configured in Jenkins. Ensure the name matches the one you set up in Jenkins.
        maven 'Maven Integration with Jenkins'
    }

    environment {
        // Name of the SonarQube server configured in Jenkins.
        SONARQUBE_SERVER = 'SonarQube'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Building the project using Maven. Adjust the command according to your project's build command.
                    echo 'Building the project...'
                    sh 'mvn clean install'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Running tests using Maven. Adjust the command according to your project's test command.
                    echo 'Running tests...'
                    sh 'mvn test'
                }
            }
        }

        stage('Code Quality Analysis') {
            steps {
                script {
                    // Running code quality analysis using SonarQube. The 'withSonarQubeEnv' ensures the SonarQube environment is available for this step.
                    echo 'Running code quality analysis...'
                    withSonarQubeEnv('SonarQube') {
                        // The Maven command for SonarQube analysis. Adjust according to your project's SonarQube command.
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploying the application to the test environment. This example uses Docker Compose.
                    echo 'Deploying to test environment...'
                    sh 'docker-compose up -d'
                }
            }
        }

        stage('Release') {
            steps {
                script {
                    // Releasing the application to production. This example uses AWS CodeDeploy.
                    echo 'Releasing to production...'
                    // Uncomment and adjust the command below according to your project's deployment requirements.
                    // sh 'aws deploy create-deployment --application-name MyApp --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name MyDeploymentGroup --description "My deployment" --github-location repository=Samraat29/Jenkins6.2HD,commitId=main'
                }
            }
        }

        stage('Monitoring and Alerting') {
            steps {
                script {
                    // Setting up monitoring and alerting for the application. This example uses Datadog.
                    echo 'Setting up monitoring and alerting...'
                    // Uncomment and adjust the command below according to your monitoring tool.
                    // sh 'datadog-agent start'
                }
            }
        }
    }

    post {
        success {
            // This message is displayed when the pipeline completes successfully.
            echo 'Pipeline completed successfully!'
        }
        failure {
            // This message is displayed when the pipeline fails.
            echo 'Pipeline failed!'
        }
    }
}
