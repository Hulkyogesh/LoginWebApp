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
                        nexusUrl: '13.233.165.224:8081',
                        repository: 'Java-Prj-Snapshot',
                        credentialsId: NEXUS_CREDENTIALS_ID,
                        groupId: pom.groupId,
                        artifactId: pom.artifactId,
                        version: pom.version,
                        packaging: 'war',
                        file: warFile
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
