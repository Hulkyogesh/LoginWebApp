@NonCPS
def getPomDetails(file) {
    def pom = new XmlSlurper().parse(new File(file))
    return [artifactId: pom.artifactId.text(), groupId: pom.groupId.text(), version: pom.version.text()]
}
 
pipeline {
    agent any
    tools {
        maven "Maven"
        jdk "Java11"
    }
 
    environment {
        NEXUS_CREDENTIALS_ID = 'NexusCred' // ID of the credentials in Jenkins
NEXUS_URL = 'http://13.200.252.130:8081/repository/Java-Prj-Snapshot/' // Nexus repository URL
    }
 
    stages {
        stage('Checkout') {
            steps {
                git branch: 'feature/nexusUpload', url:  'https://github.com/Hulkyogesh/LoginWebApp.git'; 
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Upload to Nexus') {
            steps {
                script {
                    def pomDetails = getPomDetails('pom.xml')
                    def warFile = "target/${pomDetails.artifactId}-${pomDetails.version}.war"
 
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: NEXUS_URL,
                        repository: 'Java-Prj-Snapshot',
                        credentialsId: NEXUS_CREDENTIALS_ID,
                        artifacts: [
                            [
                                artifactId: pomDetails.artifactId,
                                groupId: pomDetails.groupId,
                                version: pomDetails.version,
                                packaging: 'war',
                                file: warFile
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
has context menu
