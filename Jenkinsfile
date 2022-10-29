
pipeline {
    agent any

    environment {
        jobCredentialsId = 'admin'
        iqStage = 'build'
    }

    tools {
        jdk 'jdk8'
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
                           nexusPolicyEvaluation advancedProperties: '',
                                              enableDebugLogging: true,
                                              failBuildOnNetworkError: false,
                                              iqApplication: selectedApplication('sbootapp-ci'),
                                              iqInstanceId: "${iqStage}",
//                                               iqScanPatterns: [[scanPattern: '**/*.jar '],
//                                                                [scanPattern: '**/*.war'],
//                                                                [scanPattern: '**/*.xml']],
                                              iqScanPatterns: [[scanPattern: 'sbootapp-*.jar']],
                                              iqStage: 'build',
                                              jobCredentialsId: '${jobCredentialsId}'
                }
            }
        }
    }
}

