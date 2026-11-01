
pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t static-website:latest .'
            }
        }

        stage('Test Website') {
            steps {
                bat 'docker run -d --name static-website-test -p 8081:80 static-website:latest'
                bat 'powershell -NoProfile -Command "$r = Invoke-WebRequest -Uri http://localhost:8081 -UseBasicParsing; if ($r.StatusCode -ne 200 -or $r.Content -notmatch \'Welcome to My Website!\') { exit 1 }"'
            }
            post {
                always {
                    bat 'docker rm -f static-website-test'
                }
            }
        }

        stage('Deploy Website Locally') {
            steps {
                bat 'docker rm -f static-website-container 2>NUL || exit /b 0'
                bat 'docker run -d --name static-website-container -p 8082:80 static-website:latest'
            }
        }
    }
}
