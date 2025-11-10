pipeline {
    agent any

    environment {
        // Set up environment variables for reuse
        JF_SERVER = 'LocalArtifactory'             // JFrog server ID (the one you configured in CLI)
        JFROG_CLI = 'C:\\jfrog\\jfrog.exe'         // Path to JFrog CLI
        MAVEN_HOME = 'C:\\Program Files\\Apache\\apache-maven-3.9.9' // Update if Maven installed elsewhere
    }

    stages {

        stage('Checkout from GitHub') {
            steps {
                echo '📥 Checking out source code from GitHub...'
                checkout scm
            }
        }

        stage('Build using Maven') {
            steps {
                echo '⚙️ Running Maven clean install...'
                bat "\"${MAVEN_HOME}\\bin\\mvn.cmd\" clean install"
            }
        }

        stage('Run Tests') {
            steps {
                echo '🧪 Running Unit Tests...'
                // Optionally run: bat "\"${MAVEN_HOME}\\bin\\mvn.cmd\" test"
            }
        }

        stage('Package Application') {
            steps {
                echo '📦 Packaging JAR file...'
                bat "\"${MAVEN_HOME}\\bin\\mvn.cmd\" package"
            }
        }

        stage('Check JFrog Connection') {
            steps {
                echo '🔗 Pinging JFrog Artifactory...'
                bat "\"${JFROG_CLI}\" rt ping --server-id ${JF_SERVER}"
            }
        }

        stage('Upload Artifact to Artifactory') {
            steps {
                script {
                    echo '🚀 Uploading build artifact to JFrog Artifactory...'
                    bat "\"${JFROG_CLI}\" rt upload \"target/*.jar\" \"maven-repo-local/maven-builds/\" --server-id ${JF_SERVER}"
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build, Test, and Upload completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Please check the Jenkins console output for details.'
        }
    }
}
