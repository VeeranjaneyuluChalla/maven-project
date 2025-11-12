pipeline {
    agent any

    tools {
        maven 'Maven-3.9.9'
    }

    environment {
        JFROG_CLI_PATH = 'C:\\jfrog\\jfrog.exe'
        JF_SERVER = 'LocalArtifactory'
        ARTIFACTORY_REPO = 'maven-local' // Change if you named your repo differently
        BUILD_NAME = 'maven-project-build'
        BUILD_NUMBER = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo '📦 Checking out code from GitHub...'
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                echo '⚙️ Building the project...'
                bat 'mvn clean install'
            }
        }

        stage('Check JFrog Connection') {
            steps {
                echo '🔗 Verifying JFrog Artifactory connection...'
                bat "\"${JFROG_CLI_PATH}\" rt ping --server-id ${JF_SERVER}"
            }
        }

        stage('Upload Artifact & Publish Build Info') {
            steps {
                script {
                    echo '🚀 Uploading artifacts to JFrog and publishing build info...'
                    bat """
                        "${JFROG_CLI_PATH}" rt upload "target/*.jar" "${ARTIFACTORY_REPO}/jenkins-demo/" --server-id ${JF_SERVER} --flat=true --build-name=${BUILD_NAME} --build-number=${BUILD_NUMBER}"
                        "${JFROG_CLI_PATH}" rt build-add-git ${BUILD_NAME} ${BUILD_NUMBER}
                        "${JFROG_CLI_PATH}" rt build-publish ${BUILD_NAME} ${BUILD_NUMBER} --server-id ${JF_SERVER}
                    """
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build, Upload, and Publish completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Please check console logs for details.'
        }
    }
}
