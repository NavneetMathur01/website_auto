pipeline {
    agent any
    environment{

        DOCKER_USER_NAME="navneetmathur"

        DOCKER_IMAGE_NAME="pipeline"
        DOCKER_SERVICE_NAME="pipeline"
    }
    stages {
        stage('SCM') {
            steps {
                // get the latest changes from github repository
                // sh 'git clone https://github.com/pythoncpp/ditiss-website-demo.git'
                git 'https://github.com/NavneetMathur01/website_auto.git'
            }
        }
        stage('build docker image'){
            steps{
                sh 'docker image build -t $DOCKER_USER_NAME/$DOCKER_IMAGE_NAME .'
            }
        }
        stage('docker login'){
            steps{
                
                withCredentials([string(credentialsId: 'DOCKER_ACCESS_TOKEN', variable: 'DOCKER_ACCESS_TOKEN')]) {
                sh 'echo $DOCKER_ACCESS_TOKEN | docker login -u $DOCKER_USER_NAME --password-stdin'
            }
                
                
                
            }
        }
        stage('image push'){
            steps{
                sh 'docker image push $DOCKER_USER_NAME/$DOCKER_IMAGE_NAME'
            }
        }
        stage('reload the service'){
            steps{
                sh 'docker service update --force --image $DOCKER_USER_NAME/$DOCKER_IMAGE_NAME $DOCKER_SERVICE_NAME '
            }
        }
    }
}
