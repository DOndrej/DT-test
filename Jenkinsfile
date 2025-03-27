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
                    script {
                         def isReleaseBranch = env.BRANCH_NAME?.contains("release")
                         echo "Is Release Branch: ${isReleaseBranch}"
                         dependencyTrackPublisher(
                            artifact: 'bom-3.json',
                            projectName: "sts-admin",
                            projectVersion: "latest",
                            synchronous: true,
                            dependencyTrackApiKey: "${dtrack_apikey}",
                            projectProperties: [tags: "release/sts-3.31.0", isLatest: isReleaseBranch],
                            autoCreateProjects: true,

                         )
                    }   
                }
            }
        }
    }    
}
