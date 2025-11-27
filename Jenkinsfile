pipeline {
    agent any

    environment {
        // TODO: put your real Docker Hub repo here:
        // e.g. "akshitavidiyala/devexpppp9" or "akshitavidiyala/docker-jenkins-demo"
        IMAGE_NAME = "akshitavidiyala/devexpppp9"
        DOCKERHUB = credentials('dockerhub-creds')
    }

    stages {
        // ❌ No manual git checkout needed here –
        // Jenkins has already checked out https://github.com/akshitavidiyala/devexpppp9.git

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image: ${IMAGE_NAME}:${BUILD_NUMBER}"
                sh 'docker version'
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                echo "Logging in to Docker Hub as $DOCKERHUB_USR"
                sh '''
                echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "Pushing image $IMAGE_NAME:$BUILD_NUMBER and :latest"
                sh '''
                docker push $IMAGE_NAME:$BUILD_NUMBER
                docker tag $IMAGE_NAME:$BUILD_NUMBER $IMAGE_NAME:latest
                docker push $IMAGE_NAME:latest
                '''
            }
        }
    }

    post {
        success {
            echo "✅ SUCCESS! Image pushed: $IMAGE_NAME:$BUILD_NUMBER and :latest"
        }
        failure {
            echo "❌ Build failed."
        }
    }
}
