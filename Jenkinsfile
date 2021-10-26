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
                bat "mvn deploy"
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
