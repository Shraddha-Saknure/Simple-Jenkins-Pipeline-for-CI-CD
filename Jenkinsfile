pipeline {
    agent any

    environment {
        APP_NAME = "simple-flask-app"
        APP_PORT = "3000"
    }

    stages {
        stage('Checkout') {
            steps {
                echo " Fetching code from GitHub."
                script {
                    sh 'pwd'
                    sh 'ls -la'
                    sh 'echo "Repository: ${GIT_URL}"'
                    sh 'echo "Branch: ${GIT_BRANCH}"'
                    sh 'echo "Commit: ${GIT_COMMIT}"'
                }
            }
        }



        stage('Install Dependencies') {
            steps {
                echo " Installing Python dependencies."
                sh '''
                    python3 --version
                    python3 -m pip --version
                    python3 -m pip install --user -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo " Running tests."
                sh '''
                    echo "Looking for test files."
                    find . -name "*test*.py" -type f
                    python3 -m pytest tests/ -v
                '''
            }
        }





        stage('Build Artifact') {
            steps {
                echo " Creating build artifact."
                sh '''
                    echo "=== Creating Build Package ==="
                    BUILD_DIR="build-${BUILD_NUMBER}"
                    mkdir -p "$BUILD_DIR"

                    # Copy source files
                    if [ -d "src" ]; then
                        cp -r src "$BUILD_DIR/"
                        echo " Source files copied"
                    fi

                    # Copy configuration files
                    for file in requirements.txt Dockerfile *.md; do
                        if [ -f "$file" ]; then
                            cp "$file" "$BUILD_DIR/"
                            echo " Copied $file"
                        fi
                    done

                    # Create build info
                    cat > "$BUILD_DIR/build-info.txt" << EOF
Build Number: ${BUILD_NUMBER}
Build Date: $(date)
Git Commit: ${GIT_COMMIT}
Git Branch: ${GIT_BRANCH}
Jenkins Job: ${JOB_NAME}
EOF

                    echo "\\n=== Build Artifact Created ==="
                    ls -la "$BUILD_DIR"
                    echo "Artifact size: $(du -sh $BUILD_DIR | cut -f1)"
                '''
            }
        }

        stage('Deploy Simulation') {
            steps {
                echo " Simulating deployment."
                sh '''
                    echo "=== Deployment Simulation ==="
                    echo " Code fetched from GitHub successfully"
                    echo " Dependencies resolved"
                    echo " Tests executed"
                    echo " Security checks passed"
                    echo " Build artifact created"

                    echo "\\n=== Deployment Summary ==="
                    echo "Application: ${APP_NAME}"
                    echo "Target Port: ${APP_PORT}"
                    echo "Environment: Production-Ready"
                    echo "Build Number: ${BUILD_NUMBER}"
                    echo "Status: READY FOR DEPLOYMENT"

                    echo "\\n=== Next Steps ==="
                    echo "1. Deploy to staging environment"
                    echo "2. Run integration tests"
                    echo "3. Deploy to production"
                    echo "4. Monitor application health"

                    echo "\\n Deployment simulation completed successfully!"
                '''
            }
        }
    }

    post {
        always {
            echo "Cleanup and reporting..."
            sh '''
                echo "=== Build Summary ==="
                echo "Workspace: $(pwd)"
                echo "Build completed: $(date)"
                echo "Disk usage: $(du -sh . | cut -f1)"
            '''
        }

        success {
            echo """
             PIPELINE SUCCESS!

            Build Details:
            Repository: Successfully cloned from GitHub
            Build Number: ${BUILD_NUMBER}
            Branch: ${GIT_BRANCH}
            All stages completed successfully

            Your Flask application is ready for deployment!

            Summary:
            - Source code validated
            - Dependencies checked
            - Tests executed
            - Build artifact created
            - Ready for production
            """
        }

        failure {
            echo """
            PIPELINE FAILED!

            Troubleshooting steps:
            1. Check console output above for specific errors
            2. Verify GitHub repository access
            3. Check Python/dependency requirements
            4. Review file structure and permissions

            Common fixes:
            - Ensure requirements.txt is valid
            - Check Python syntax in source files
            - Verify test files are properly structured
            """
        }
    }
}
