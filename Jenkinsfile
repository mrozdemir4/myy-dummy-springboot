pipeline {
    agent { label "master" }
    environment {
        APP_REPO_NAME= "my-spring-boot"
        PATH="/usr/local/bin/:${env.PATH}"
    }
    stages {
        stage('Build Docker Compile image') {
            steps {
                sh 'docker run --rm -v $HOME/.m2:/root/.m2 -v $WORKSPACE:/app -w /app maven:3.6.3-openjdk-8 mvn clean package'
                //sh 'docker image ls'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ozdemire/lastspringboot .'
                sh 'docker image ls'
            }
        }
        stage('Push Image to DockerHub') {
            steps {
                sh 'docker login -u ozdemire -p 3154500Emre.'
                sh 'docker push ozdemire/lastspringboot'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker login -u ozdemire -p 3154500Emre.'
                //sh 'kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.35.0/deploy/static/provider/aws/deploy.yaml'
                sh 'kubectl apply -f k8s'
            }
        }

    }
    post {
        always {
            echo 'Deleting all local images'
            sh 'docker image prune -af'
        }
    }
}