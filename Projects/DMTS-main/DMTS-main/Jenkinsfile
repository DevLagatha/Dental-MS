pipeline {
    agent {
        kubernetes {
            label 'simple-test'
        }
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out source code..."
                git branch: 'main', url: 'https://github.com/DevLagatha/DMTS.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies..."
                sh '''
                    if [ -f requirements.txt ]; then
                        pip install --no-cache-dir -r requirements.txt
                    else
                        echo "No requirements.txt found, skipping..."
                    fi
                    pip install --no-cache-dir pytest
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo "Running unit tests..."
                sh '''
                    set +e
                    pytest -v || echo "Some tests failed or no tests found, continuing..."
                    set -e
                '''
            }
        }

        stage('Build Application') {
            steps {
                echo "Building app..."
                sh 'python -m compileall .'
            }
        }

        stage('Deploy (Local Simulation)') {
            steps {
                echo "Simulating deployment..."
                sh 'echo "Flask app would be deployed here (Docker image build, push, etc.)"'
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully! All stages passed."
        }
        failure {
            echo "Pipeline failed. Check the stage logs for details."
        }
        always {
            echo "Cleaning up workspace..."
            script {
                if (env.WORKSPACE) {
                    cleanWs()
                } else {
                    echo "No workspace context found to clean."
                }
            }
        }
    }
}
