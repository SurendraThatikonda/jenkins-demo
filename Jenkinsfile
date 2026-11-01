pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out Java source code'
            }
        }

        stage('Compile') {
            steps {
                bat 'javac HelloJenkins.java'
            }
        }

        stage('Execute') {
            steps {
                bat 'java HelloJenkins'
            }
        }

    }
}