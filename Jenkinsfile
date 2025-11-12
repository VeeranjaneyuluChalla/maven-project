pipeline {
    agent any

    tools {
        maven 'Maven-3.9.9'
    }

    environment {
        JFROG_CLI_PATH = 'C:\\jfrog\\jfrog.exe'
        JF_SERVER = 'LocalArtifactory'
        ARTIFACTORY_REPO = 'maven-local' // Change if you use another repo name
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo '📦 Checking out code from GitHub repository...'
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                echo '⚙️ Building the project using Maven...'
                bat 'mvn clean install'
            }
        }

        stage('Check JFrog Connection') {
            steps {
                echo '🔗 Pinging JFrog Artifactory server...'
                bat "\"${JFROG_CLI_PATH}\" rt ping --server-id ${JF_SERVER}"
            }
        }

        stage('Upload Artifact to Artifactory') {
            steps {
                echo '🚀 Uploading JAR file to Artifactory...'
                bat """
                    dir target
                    "${JFROG_CLI_PATH}" rt upload "target/*.jar" "${ARTIFACTORY_REPO}/jenkins-demo/" --server-id ${JF_SERVER} --flat=true
                """
            }
        }
    }

    post {
        success {
            echo '✅ Build and Upload completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Please check the console output for details.'
        }
    }
}
