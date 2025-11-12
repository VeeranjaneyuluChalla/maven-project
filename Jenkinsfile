pipeline {
    agent any

    // 🔧 Tools Section
    // Jenkins will automatically add Maven to PATH using the configured name in "Global Tool Configuration"
    tools {
        maven 'Maven-3.9.9'
    }

    // 🌍 Environment Variables
    environment {
        JFROG_CLI_PATH = 'C:\\jfrog\\jfrog.exe'     // Path to your installed JFrog CLI
        JF_SERVER = 'LocalArtifactory'               // Server ID configured with jfrog c add
        REPO = 'maven-releases'                      // Local repository name in Artifactory
    }

    stages {

        // 🧾 1️⃣ Checkout Code from GitHub
        stage('Checkout Code') {
            steps {
                echo "📦 Checking out source code from GitHub repository..."
                checkout scm
            }
        }

        // ⚙️ 2️⃣ Build the Project with Maven
        stage('Build with Maven') {
            steps {
                echo "⚙️ Building the project using Maven..."
                bat 'mvn clean install'
            }
        }

        // 🔗 3️⃣ Check Connection to JFrog Artifactory
        stage('Check JFrog Connection') {
            steps {
                echo "🔗 Pinging JFrog Artifactory server..."
                bat "\"${JFROG_CLI_PATH}\" rt ping --server-id ${JF_SERVER}"
            }
        }

        // 📤 4️⃣ Upload Artifact to JFrog Artifactory
        stage('Upload Artifact to Artifactory') {
            steps {
                echo "📤 Uploading the JAR file to JFrog Artifactory..."
                bat """
                    dir target
                    "${JFROG_CLI_PATH}" rt upload "target/*.jar" "${REPO}/" --server-id ${JF_SERVER}
                """
            }
        }
    }

    // 🧩 5️⃣ Post Actions
    post {
        success {
            echo "✅ Build and artifact upload completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Please check the Jenkins console output for details."
        }
    }
}
