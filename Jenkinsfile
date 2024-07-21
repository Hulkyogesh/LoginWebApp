pipeline {
    agent any
        tools {
        maven "Maven"
        jdk "Java11"
    }

    environment {
        NEXUS_CREDENTIALS_ID = 'NexusCred' // ID of the credentials in Jenkins
        NEXUS_URL = 'http://13.233.165.224:8081/repository/Java-Prj-Snapshot/' // Nexus repository URL
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Hulkyogesh/LoginWebApp.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean deploy'
            }
        }
        stage('Upload to Nexus') {
            steps {
                script {
                    def pom = readMavenPom file: 'pom.xml'
                    def warFile = "target/${pom.artifactId}-${pom.version}.war"
                    
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: NEXUS_URL,
                        groupId: pom.groupId,
                        version: pom.version,
                        repository: 'maven-releases',
                        credentialsId: NEXUS_CREDENTIALS_ID,
                        artifacts: [
                            [
                                artifactId: pom.artifactId,
                                classifier: '',
                                file: warFile,
                                type: 'war'
                            ]
                        ]
                    )
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
