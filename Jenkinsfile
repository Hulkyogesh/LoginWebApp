pipeline {
    agent any
    tools {
        maven "Maven"
        jdk "Java"
    }

     environment {
        NEXUS_CREDENTIALS = credentials('nexuscred')
     }

    stages {
        stage("Check out") {
            steps {
                script {
                    git branch: 'feature/nexusUpload', url:  'https://github.com/Hulkyogesh/LoginWebApp.git';
                }
            }
        }

        stages{
        stage('Build'){
            steps{
                 sh script: 'mvn clean package'
                 archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
            }
        }
        stage('Upload War To Nexus'){
            steps{
                script{

                    def mavenPom = readMavenPom file: 'pom.xml'
                    def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "LoginWebApp-snapshot" : "LoginWebApp-release"
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'simple-app', 
                            classifier: '', 
                            file: "target/LoginWebApp Maven Webapp-${mavenPom.version}.war", 
                            type: 'war'
                        ]
                    ], 
                    credentialsId: 'nexuscred', 
                    groupId: 'com.ranjitswain', 
                    nexusUrl: '13.233.193.184:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: nexusRepoName, 
                    version: "${mavenPom.version}"
                    }
            }
        }
    }
}
