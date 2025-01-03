pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Creating virtual environment and installing dependencies...'
                bat '''
                python -m venv venv
                .\\venv\\Scripts\\activate
                pip install -r requirements.txt
                '''
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                bat '''
                .\\venv\\Scripts\\activate
                python -m unittest discover -s .
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                bat '''
                mkdir -p ${WORKSPACE}\\python-app-deploy
                copy ${WORKSPACE}\\app.py ${WORKSPACE}\\python-app-deploy
                '''
            }
        }
        stage('Run Application') {
            steps {
                echo 'Running application...'
                bat '''
                nohup python ${WORKSPACE}\\python-app-deploy\\app.py > ${WORKSPACE}\\python-app-deploy\\app.log 2>&1 &
                echo %! > ${WORKSPACE}\\python-app-deploy\\app.pid
                '''
            }
        }
        stage('Test Application') {
            steps {
                echo 'Testing application...'
                bat '''
                python ${WORKSPACE}\\test_app.py
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
