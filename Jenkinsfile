pipeline {
    agent any
    stages {
        stage('Cleaning Stage') {
            steps {
                bat "mvn clean"
            }
        }
        stage('Testing Stage') {
            steps {
                bat "mvn test"
            }
        }
        stage('Packaging Stage') {
            steps {
                bat "mvn package"
            }
        }
        stage("Deploy") {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'Maven', 
                                                   classifier: '', 
                                                   file: 'target/Maven-0.0.1-SNAPSHOT.jar', 
                                                   type: 'jar'
                                                  ]
                                                 ], 
                    credentialsId: 'dd59719f-8d10-4fa9-a0be-425bcd8b6318',
                    groupId: 'Jenkins', 
                    nexusUrl: 'localhost:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'jenkins-snapshots', 
                    version: '0.0.1-SNAPSHOT'
            }
        }
                stage('SonarQube Stage') {
            steps {
                withSonarQubeEnv('SonarQubeDefault')
                {
                    bat "mvn sonar:sonar"
                }
            }
        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
