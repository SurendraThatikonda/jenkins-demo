pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Source code checked out from GitHub'
            }
        }

        stage('Build with Maven') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Run Java Application') {
            steps {
                bat 'java -cp target\\jenkins-practice-1.0-SNAPSHOT.jar HelloJenkins'
            }
        }

    }

    post {
        success {
            echo 'Maven build completed successfully!'
        }
        failure {
            echo 'Build failed. Please check the console output.'
        }
    }
}
