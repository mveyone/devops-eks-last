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
          git branch: 'main', credentialsId: 'github', url: 'https://github.com/mveyone/devops-eks-last.git'
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
        // stage('deployment of nodejsapp on eks'){
        //   steps{
        //     sh 'ansible-playbook k8s/k8s-playbook.yml'
        //   }
        // }
        stage('Update Kubeconfig') {
          steps {
            // You will need to install CloudBees AWS Credentials Plugin in Jenkins and add AWS Credentials first 
              withCredentials([[
                $class: 'AmazonWebServicesCredentialsBinding',
                credentialsId: "aws-credentials",
                accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]) {
                sh """
                aws eks update-kubeconfig --name k8s-eks
                """
              }
          }
      }

      stage('Deploy App') {
          steps {
            // You will need to install CloudBees AWS Credentials Plugin in Jenkins and add AWS Credentials first 
              withCredentials([[
                $class: 'AmazonWebServicesCredentialsBinding',
                credentialsId: "aws-credentials",
                accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]) {
                sh " ansible-playbook k8s-playbook.yml "
                sleep(time: 120, unit: "SECONDS")
              }
          }
      }
      stage('Reveal APP URL') {
          steps {
            // You will need to install CloudBees AWS Credentials Plugin in Jenkins and add AWS Credentials first 
              withCredentials([[
                $class: 'AmazonWebServicesCredentialsBinding',
                credentialsId: "aws-credentials",
                accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]) {
                sh "aws elb describe-load-balancers | grep DNSName"
              }
          }
      }
    }
 }
