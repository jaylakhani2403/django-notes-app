@Library('Shared') _
pipeline {
    agent { label 'vinod' }

    stages {
        stage('Code') {
            steps {
                script{
                    git_clone("https://github.com/jaylakhani2403/django-notes-app.git","main")
                }
            }
        }

        stage('Build') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t notes-app:latest ."
            }
        }

        stage('Push Docker hub') {
            steps {
                echo "Push image..."

                withCredentials([usernamePassword(
                    credentialsId: "dockerhubCred",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                )]) {

                    sh '''
                    echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin
                    docker tag notes-app:latest jaylakhani2403/notes-app:latest
                    docker push jaylakhani2403/notes-app:latest
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying with Docker Compose..."

                sh '''
                docker compose down || true
                docker compose up -d --build
                '''
            }
        }
    }
}
