pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub'
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

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar',
                                 fingerprint: true
            }
        }

        stage('Deploy to Nexus') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'nexus-credentials',
                        usernameVariable: 'NEXUS_USER',
                        passwordVariable: 'NEXUS_PASSWORD'
                    )
                ]) {
                    script {
                        def xmlEscape = { value ->
                            value.replace('&', '&amp;')
                                 .replace('<', '&lt;')
                                 .replace('>', '&gt;')
                                 .replace('"', '&quot;')
                                 .replace("'", '&apos;')
                        }

                        writeFile file: 'nexus-settings.xml',
                            text: """<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.2.0">
    <servers>
        <server>
            <id>nexus-snapshots</id>
            <username>${xmlEscape(env.NEXUS_USER)}</username>
            <password>${xmlEscape(env.NEXUS_PASSWORD)}</password>
        </server>
        <server>
            <id>nexus-releases</id>
            <username>${xmlEscape(env.NEXUS_USER)}</username>
            <password>${xmlEscape(env.NEXUS_PASSWORD)}</password>
        </server>
    </servers>
</settings>
"""

                        try {
                            bat 'mvn -B -s nexus-settings.xml deploy -DskipTests'
                        } finally {
                            bat 'if exist nexus-settings.xml del /q nexus-settings.xml'
                        }
                    }
                }
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
            junit allowEmptyResults: true,
                  testResults: 'target/surefire-reports/*.xml'
        }

        success {
            echo 'Build, tests and Nexus deployment completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}
