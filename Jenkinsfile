pipeline {
    agent any

    stages {
        // STAGE 1: Checkout code
        stage('Checkout Code') {
            steps {
                echo 'Fetching code from GitHub...'
                checkout scm
            }
        }

        // STAGE 2: Check files
        stage('Check Files') {
            steps {
                echo 'Checking repository files...'
                bat '''
                    echo === FILES IN REPOSITORY ===
                    dir
                    echo.
                    echo === CHECKING OUR FILES ===

                    if exist Dockerfile (
                        echo Dockerfile found
                        echo Dockerfile contents:
                        more +0 Dockerfile
                    ) else (
                        echo Dockerfile not found
                    )

                    if exist requirements.txt (
                        echo requirements.txt found
                    ) else (
                        echo requirements.txt not found
                    )

                    if exist src\\app.py (
                        echo File src\\app.py found
                    ) else (
                        echo File src\\app.py not found
                    )

                    if exist .dockerignore (
                        echo .dockerignore found
                    ) else (
                        echo .dockerignore not found
                    )
                '''
            }
        }

        // STAGE 3: Docker theory
        stage('Docker Theory') {
            steps {
                echo 'DOCKER THEORY'
                echo 'If Jenkins was not on Windows, we would do:'
                echo '1. docker build -t my-app .'
                echo '2. docker run -d -p 5000:5000 my-app'
                echo '3. curl http://localhost:5000/health'
                echo 'On Windows, commands run via bat if Docker Desktop is running.'
            }
        }

        // STAGE 4: Try Docker commands
        stage('Try Docker Commands') {
            steps {
                echo 'Trying Docker commands...'
                bat '''
                    echo Checking Docker version
                    docker --version || echo Docker not available

                    echo Building Docker image
                    docker build -t flask-lab2 . || echo Failed to build image

                    echo Running Docker container
                    docker run -d -p 5000:5000 --name flask-lab2-container flask-lab2 || echo Failed to run container
                '''
            }
        }
    }

    post {
        always {
            echo '=== JOB RESULT ==='
            bat '''
                echo Date: %date% %time%
                echo CI/CD process understood?
                echo 1. Code → GitHub
                echo 2. GitHub → Jenkins
                echo 3. Jenkins checks files
                echo 4. Docker theory done
            '''
        }
        success {
            echo 'GREAT! You understood the CI/CD process!'
            echo 'Even if Docker did not run — well done!'
        }
        failure {
            echo 'There were errors, but this is part of learning!'
            echo 'Main thing — files exist and process is clear.'
        }
    }
}
