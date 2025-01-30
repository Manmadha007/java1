pipeline {
    agent {
        docker {
            image 'maven:3.9.0'
            args '-v /root/.m2:/root/.m2' // Maps the Maven repository directory for caching dependencies
        }
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'mvn -B -DskipTests clean package' // Batch mode with skipped tests for packaging
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test' // Executes the unit tests
            }
            post {
                always {
                    echo 'Archiving test results...'
                    junit 'target/surefire-reports/*.xml' // Collects test results for reporting
                }
            }
        }
        stage('Deliver') {
            steps {
                echo 'Delivering the application...'
                sh './jenkins/scripts/deliver.sh' // Custom delivery script
            }
        }
    }
    post {
        always {
            echo 'Cleaning up the workspace...'
            cleanWs() // Cleans up the workspace after execution
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
