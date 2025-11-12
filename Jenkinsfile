pipeline {
    agent any

    environment {
        JFROG_CLI_PATH = 'C:\\jfrog\\jfrog.exe'
        JF_SERVER = 'LocalArtifactory'
        REPO = 'maven-releases'  // Make sure you create this repo in Artifactory
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "📦 Checking out code from GitHub..."
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                echo "⚙️ Building the project with Maven..."
                bat 'mvn clean install'
            }
        }

        stage('Check JFrog Connection') {
            steps {
                echo "🔗 Pinging JFrog Artifactory..."
                bat "\"${JFROG_CLI_PATH}\" rt ping --server-id ${JF_SERVER}"
            }
        }

        stage('Upload Artifact to Artifactory') {
            steps {
                echo "📤 Uploading JAR file to Artifactory..."
                bat """
                    dir target
                    "${JFROG_CLI_PATH}" rt upload "target/*.jar" "${REPO}/" --server-id ${JF_SERVER}
                """
            }
        }
    }

    post {
        success {
            echo "✅ Build and Upload Successful!"
        }
        failure {
            echo "❌ Pipeline failed. Please check the console output for details."
        }
    }
}
