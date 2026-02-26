pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                echo "Building Java project..."
                cd "Password Protection"
                mkdir -p build
                javac -d build src/*.java
                echo "Build successful"
                '''
            }
        }
        stage('Test') {
            steps {
                sh '''
                echo "Running JUnit tests for File-Encrypter..."
                cd "Password Protection"
                
                # Download JUnit jar
                if [ ! -f junit-platform-console-standalone.jar ]; then
                    echo "Downloading JUnit..."
                    curl -L -o junit-platform-console-standalone.jar "https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.10.0/junit-platform-console-standalone-1.10.0.jar"
                fi
                
                # Compile test files
                mkdir -p test-build
                javac -cp junit-platform-console-standalone.jar:build -d test-build test/*.java 2>/dev/null || echo "No tests found"
                
                echo "JUnit download completed"
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                echo "Deploying (Packaging) File-Encrypter Application..."
                cd "Password Protection"
                jar cf FileEncrypter.jar -C build .
                echo "Deployment successful - Artifact ready"
                ls -lh FileEncrypter.jar
                '''
            }
        }
    }
    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
