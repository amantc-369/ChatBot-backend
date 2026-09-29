pipeline {
agent any

```
environment {
    DOCKER_IMAGE = "amantc/chatbot-backend"
}

stages {

    stage('Checkout') {
        steps {
            git branch: 'main',
                url: 'https://github.com/amantc-369/ChatBot-backend.git'
        }
    }

    stage('Install Dependencies') {
        steps {
            sh '''
                export PATH="$HOME/.bun/bin:$PATH"
                bun install
            '''
        }
    }

    stage('Test') {
        steps {
            sh '''
                export PATH="$HOME/.bun/bin:$PATH"
                bunx tsc --noEmit
            '''
        }
    }

    stage('Build Docker Image') {
        steps {
            sh '''
                docker build -t $DOCKER_IMAGE:latest .
            '''
        }
    }

    stage('Push to Docker Hub') {
        steps {
            withCredentials([
                usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )
            ]) {
                sh '''
                    echo "$DOCKER_PASSWORD" | docker login \
                        -u "$DOCKER_USERNAME" \
                        --password-stdin

                    docker push $DOCKER_IMAGE:latest

                    docker logout
                '''
            }
        }
    }
}

post {
    success {
        echo 'ChatBot pipeline completed successfully!'
    }

    failure {
        echo 'ChatBot pipeline failed.'
    }
}
```

}

