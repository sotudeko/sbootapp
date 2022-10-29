
pipeline {
    agent any

    environment {
        jobCredentialsId = 'admin'
        iqStage = 'build'
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -Dproject.version=$BUILD_VERSION -Dmaven.test.failure.ignore clean deploy'
            }
        }

        stage('Nexus IQ Scan'){
            steps {
                script{
                        nexusPolicyEvaluation failBuildOnNetworkError: true, 
                                              iqApplication: selectedApplication('sbootapp-ci'),
                                              iqScanPatterns: [[scanPattern: '**/*.jar']],
                                              iqStage: "${iqStage}", 
                                              jobCredentialsId: "${jobCredentialsId}"
                }
            }
        }
    }
}

