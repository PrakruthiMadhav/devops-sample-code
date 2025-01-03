pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Creating virtual environment and installing dependencies...'
                bat '''
                python -m venv venv
                call venv\\Scripts\\activate
                pip install Flask
                '''
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                bat '''
                call venv\\Scripts\\activate
                python -m unittest discover -s tests -p "test_*.py"
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                bat '''
                mkdir %WORKSPACE%\\python-app-deploy
                copy %WORKSPACE%\\app.py %WORKSPACE%\\python-app-deploy\\
                '''
            }
        }
        stage('Run Application') {
            steps {
                echo 'Running application...'
                bat '''
                call venv\\Scripts\\activate
                python %WORKSPACE%\\python-app-deploy\\app.py > %WORKSPACE%\\python-app-deploy\\app.log 2>&1
                '''
            }
        }
        stage('Test Application') {
            steps {
                echo 'Testing application...'
                bat '''
                call venv\\Scripts\\activate
                python %WORKSPACE%\\tests\\test_app.py
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for more details.'
        }
    }
}
