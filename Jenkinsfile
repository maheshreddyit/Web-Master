pipeline {
    agent any
    tools {
        maven 'Maven363'
    }
    options {
        timeout(10)
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')
    }
    stages {
        stage('Build') {
            steps {
                sh "mvn clean install"
            }
        }
        stage('Upload Artifact to Nexus') {
            steps {
                nexusArtifactUploader(
                    artifacts: [
                        [
                            artifactId: 'wwp', 
                            classifier: '', 
                            file: 'target/wwp-1.0.0.war', 
                            type: 'war'
                        ]
                    ], 
                    credentialsId: 'nexus3', 
                    groupId: 'koddas.web.war', 
                    nexusUrl: 'http://13.201.185.42:8080/', // Ensure URL has a scheme (http://)
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'samplerepo', 
                    version: '1.0.0'
                )
            }
        }
    }
    post {
        always {
            deleteDir()
        }
        failure {
            echo "sendmail -s mvn build failed recipients@my.com"
        }
        success {
            echo "The job is successful"
        }
    }
}

