pipeline {
    agent any

    tools {
        maven 'Maven3.9'
        jdk 'JDK17'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the Java application with Maven...'
                bat '"%MAVEN_HOME%\\bin\\mvn.cmd" clean compile'
            }
        }

        stage('Unit Test') {
            steps {
                echo 'Running unit tests...'
                bat '"%MAVEN_HOME%\\bin\\mvn.cmd" test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the application...'
                bat '"%MAVEN_HOME%\\bin\\mvn.cmd" package -DskipTests'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
