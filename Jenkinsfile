pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh '''
                echo "Building Java project..."
                echo "Listing workspace contents:"
                ls

                cd "Password Protection"

                mkdir -p build

                # Compile Java source files
                javac -d build src/Cryptoalgo.java src/Main.java

                echo "Build successful"
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                echo "Running JUnit tests for File-Encrypter..."
                cd "Password Protection"

                # Remove old/corrupted JUnit jar if it exists
                rm -f junit-platform-console-standalone.jar

                echo "Downloading JUnit platform console..."
                curl -L -o junit-platform-console-standalone.jar https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.10.0/junit-platform-console-standalone-1.10.0.jar

                echo "JUnit download completed"

                # If test directory exists, compile and run tests
                if [ -d test ]; then
                    echo "Test directory found. Compiling tests..."
                    mkdir -p test-build
                    javac -cp junit-platform-console-standalone.jar:build -d test-build test/*.java

                    echo "Running JUnit tests..."
                    java -jar junit-platform-console-standalone.jar --class-path build:test-build --scan-class-path
                else
                    echo "No test directory found. Skipping test stage."
                fi

                echo "Test stage completed"
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                echo "Creating JAR file..."
                cd "Password Protection"

                mkdir -p deploy

                jar cvf deploy/File-Encrypter.jar -C build .

                echo "JAR file created successfully"
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
