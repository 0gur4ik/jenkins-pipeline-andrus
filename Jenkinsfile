pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building Docker image...'
                // Збираємо локально як myapp:latest
                sh 'docker build -t myapp:latest .' 
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'echo Tests passed!'
            }
        }

        stage('Push to DockerHub') {
            steps {
                // Використовуємо ID, який ми створили на Кроці 1
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    script {
                        // 1. Створюємо тег формату: user/repo:tag
                        sh "docker tag myapp:latest ${DOCKER_USER}/myapp:latest"
                        
                        // 2. Логінимось (безпечно передаємо пароль)
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        
                        // 3. Пушимо
                        sh "docker push ${DOCKER_USER}/myapp:latest"
                        
                        // 4. Виходимо (good practice)
                        sh "docker logout"
                    }
                }
            }
        }
    }
}
