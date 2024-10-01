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
                sh 'docker pull noveedwork/activity4:app'
                sh 'docker pull noveedwork/activity4:server'
            }
        }
        stage("Running the container"){
            steps{
                sh 'docker run -d --name app --network new-network noveedwork/activity4:app'
                sh 'docker run -d -p 80:80 --name server --network new-network noveedwork/activity4:server'
            }
        }
    }
}