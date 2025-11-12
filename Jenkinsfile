pipeline {
    agent any

    tools {
        maven 'Maven-3.9.9' // Use your configured Maven installation name in Jenkins
    }

    environment {
        JF_SERVER = 'LocalArtifactory' // JFrog Server ID from your CLI config
        JF_PATH = 'C:\\jfrog\\jfrog.exe' // Full path to jfrog.exe
        ARTIFACT_REPO = 'maven-repo-local' // JFrog repo where artifacts will be uploaded
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo '📦 Checking out code from GitHub repository...'
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/VeeranjaneyuluChalla/maven-project.git']]
                ])
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
                bat "\"${env.JF_PATH}\" rt ping --server-id ${env.JF_SERVER}"
            }
        }

        stage('Upload Artifact to Artifactory') {
            steps {
                echo '🚀 Uploading artifact to JFrog Artifactory...'
                bat "\"${env.JF_PATH}\" rt upload \"target/*.jar\" \"${env.ARTIFACT_REPO}/\" --server-id ${env.JF_SERVER}"
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
