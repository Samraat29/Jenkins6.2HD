pipeline {
    agent any

    tools {
        // Use the Maven tool configured in Jenkins. Ensure the name matches exactly as configured in Jenkins.
        maven 'Maven Integration with Jenkins'
    }

    environment {
        // Name of the SonarQube server configured in Jenkins.
        SONARQUBE_SERVER = 'SonarQube' // Replace 'SonarQube' with the exact name from the Jenkins configuration if different
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Building the project using Maven. Adjust the command according to your project's build command.
                    echo 'Building the project...'
                    dir('Jenkins6.2HD') { // Ensure this matches the relative path to your pom.xml
                        bat 'mvn clean install'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Running tests using Maven. Adjust the command according to your project's test command.
                    echo 'Running tests...'
                    dir('Jenkins6.2HD') { // Ensure this matches the relative path to your pom.xml
                        bat 'mvn test'
                    }
                }
            }
        }

        stage('Code Quality Analysis') {
            steps {
                script {
                    // Running code quality analysis using SonarQube. The 'withSonarQubeEnv' ensures the SonarQube environment is available for this step.
                    echo 'Running code quality analysis...'
                    dir('Jenkins6.2HD') { // Ensure this matches the relative path to your pom.xml
                        withSonarQubeEnv('SonarQube') { // Ensure 'SonarQube' matches the name in Jenkins configuration
                            // The Maven command for SonarQube analysis. Adjust according to your project's SonarQube command.
                            bat 'mvn sonar:sonar'
                        }
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploying the application to the test environment. This example uses Docker Compose.
                    echo 'Deploying to test environment...'
                    dir('Jenkins6.2HD') { // Ensure this matches the relative path to your docker-compose.yml
                        bat 'docker-compose up -d'
                    }
                }
            }
        }

        stage('Release') {
            steps {
                script {
                    // Releasing the application to production. This example uses AWS CodeDeploy.
                    echo 'Releasing to production...'
                    // Uncomment and adjust the command below according to your project's deployment requirements.
                    // dir('Jenkins6.2HD') { // Ensure this matches the relative path if necessary
                    //     bat 'aws deploy create-deployment --application-name MyApp --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name MyDeploymentGroup --description "My deployment" --github-location repository=Samraat29/Jenkins6.2HD,commitId=main'
                    // }
                }
            }
        }

        stage('Monitoring and Alerting') {
            steps {
                script {
                    // Setting up monitoring and alerting for the application. This example uses Datadog.
                    echo 'Setting up monitoring and alerting...'
                    // Uncomment and adjust the command below according to your monitoring tool.
                    // dir('Jenkins6.2HD') { // Ensure this matches the relative path if necessary
                    //     bat 'datadog-agent start'
                    // }
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
