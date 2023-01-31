pipeline {
    agent any

//Add Dockerhub Credentials
    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	}
  //Pipeline stages
    stages {
        stage('git checkout'){
          steps{
          git branch: 'main', credentialsId: 'github', url: 'https://github.com/mveyone/devops-.git'
          }
        }
      // First Stage
        stage('docker-login') {
            steps {
              // Command to login using dockerhub credentials  
              sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('build docker image'){
            steps{
              sh 'docker build -t $JOB_NAME:v1.$BUILD_ID . '
             }
          }
    
        stage('image tagging '){
            steps{
             sh 'docker image tag $JOB_NAME:v1.$BUILD_ID mveyone/$JOB_NAME:v1.$BUILD_ID '
             sh 'docker image tag $JOB_NAME:v1.$BUILD_ID mveyone/$JOB_NAME:latest '
            }
          }
        
        stage('Push docker image to docker hub'){
            steps{
                   sh '  docker image push mveyone/$JOB_NAME:v1.$BUILD_ID'
                   sh '  docker image push mveyone/$JOB_NAME:latest '
                   sh '  docker image rm mveyone/$JOB_NAME:latest  mveyone/$JOB_NAME:v1.$BUILD_ID $JOB_NAME:v1.$BUILD_ID'
                   }
        }
        stage('deployment of nodejsapp on eks'){
          steps{
            sh 'ansible-playbook k8s/k8s-playbook.yml'
          }
        }
    }
}