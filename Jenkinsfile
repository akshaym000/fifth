pipeline {
    agent any

    environment {
        IMAGE = "akshaym000/my-python-app:latest"
    }

    stages {

        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE .'
            }
        }

        stage('Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push') {
            steps {
                sh 'docker push $IMAGE'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f myapp-container || true
                docker run -d -p 5000:5000 --name myapp-container $IMAGE
                '''
            }
        }
    }
}
