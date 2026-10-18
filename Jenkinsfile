
pipeline {
    agent any

    options {
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/mounika892/jenkins-apache-deployment.git'
            }
        }

        stage('Validate Files') {
            steps {
                sh '''
                    test -f index.html
                    echo "Application files verified"
                    ls -la
                '''
            }
        }

        stage('Deploy to Apache') {
            steps {
                sh '''
                    set -eu
                    test -f index.html

                    find . -maxdepth 1 -type f \
                      ! -name 'Jenkinsfile' \
                      ! -name '.gitignore' \
                      -exec install -m 664 -t /var/www/html {} +

                    echo "Deployment completed successfully"
                '''
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Application deployed to Apache2.'
        }

        failure {
            echo 'FAILED: Check the Jenkins console output.'
        }
    }
}
