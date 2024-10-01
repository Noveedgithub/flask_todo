pipeline{
    agent any
    stages{
        stage("Cleanup"){
            steps{
                sh 'docker rm -f \$(docker ps -aq) || true'
                sh 'docker rmi -f \$(docker images) || true'
                sh 'docker network create new-network || true'
            }
        }
        stage("Build image"){
            steps{
                sh 'docker build -t noveed-work/todo-app:app .'
                sh 'docker build -t noveed-work/todo-app:server -f Dockerfile.nginx .'
            }
        }
        stage("Running the container"){
            steps{
                sh 'docker run -d --name todo-app --network new-network noveed-work/todo-app:app'
                sh 'docker run -d -p 80:80 --name mynginx --network new-network noveed-work/todo-app:server'
            }
        }
    }
}
