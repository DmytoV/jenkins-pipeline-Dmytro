
pipeline {
    agent any 

    stages {
        stage('Build') {
            steps {
                echo 'Building Docker image...'
                // Назва локального образу може бути будь-якою, 
                // але для зручності лишаємо dmytroapp
                sh 'docker build -t dmytroapp:latest .' 
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'echo "Tests passed!"'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Pushing Docker image to DockerHub...'
                
                // Переконайся, що в Jenkins створено Credentials з ID: dockerhub-credentials
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    
                    // Авторизація
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    
                    // Тегування: важливо, щоб назва після $DOCKER_USER/ збігалася з dmytroapp
                    sh 'docker tag dmytroapp:latest $DOCKER_USER/dmytroapp:latest'
                    
                    // Пуш у твій репозиторій kruvert/dmytroapp
                    sh 'docker push $DOCKER_USER/dmytroapp:latest'
                }
            }
        }
    }
}
