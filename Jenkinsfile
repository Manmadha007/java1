pipeline {
    agent any
    tools {
        maven 'Maven' // Ensure Maven is configured in Jenkins
        jdk 'JDK'     // Ensure JDK is configured in Jenkins
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test'
            }
            post {
                always {
                    echo 'Publishing test results...'
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                echo 'Delivering the application...'
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs.'
        }
    }
}
