pipeline {
    agent any

    tools {
        maven 'maven 3.6.1'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh "mvn --version"
            }
        }
        stage('Checkout') {
            steps {
                    checkout scm
            }
        }

        stage('Dependency track analysis') {
            steps {
                withCredentials([string(credentialsId: 'dtrack_apikey', variable: 'dtrack_apikey')]) {
                        dependencyTrackPublisher(
                            artifact: 'bom-3.json',
                            projectName: "sts-admin",
                            projectVersion: "1.1.0-RELEASE",
                            synchronous: true,
                            dependencyTrackApiKey: "${dtrack_apikey}",
                            projectProperties: [tags: "release/sts-3.31.0", isLatest: true],
                            autoCreateProjects: true,

                        )
                }
            }
        }
    }    
}
