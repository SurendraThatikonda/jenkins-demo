pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Source code checked out from GitHub'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean compile'
            }
        }

        stage('Unit Testing') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Package') {
            steps {
                bat 'mvn package -DskipTests'
            }
        }

        stage('Run Application') {
            steps {
                bat 'java -cp target\\jenkins-practice-1.0-SNAPSHOT.jar HelloJenkins'
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
        success {
            echo 'Build and unit tests completed successfully!'
        }
        failure {
            echo 'Build or unit tests failed. Check the console output.'
        }
    }
}
