pipeline {
    agent any
 
    environment {
        // Define your Nexus repository credentials and URL
        NEXUS_URL = 'http://13.201.54.145:8081/'
        NEXUS_CREDENTIAL_ID = 'nexusCredential'
    }
 
    stages {
        stage('Checkout') {
            steps {
                // Checkout your Git repository
                git 'https://github.com/Hulkyogesh/LoginWebApp.git'
            }
        }
 
        stage('Build') {
            steps {
                // Build your Maven project
                sh 'mvn clean install'
            }
        }
 
        stage('Deploy to Nexus') {
            steps {
                // Deploy the built artifact to Nexus
                script {
                    def server = Artifactory.server NEXUS_URL
                    def deployer = server.deployer NEXUS_CREDENTIAL_ID
                    deployer.deploy(
                        file: 'target/your-artifact.jar',
                        repository: 'http://13.201.54.145:8081/repository/Java-Project-snapshot/',
                        path: 'path/in/nexus'
                    )
                }
            }
        }
    }
 
    post {
        success {
            echo 'Build and deployment to Nexus succeeded!'
        }
        failure {
            echo 'Build or deployment to Nexus failed.'
        }
    }
}
