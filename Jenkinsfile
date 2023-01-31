pipeline {
    agent any

    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub')
	}
    stages {
        stage('Docker Login') {
                         // Add --password-stdin to run docker login command non-interactively
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        stage('git checkout'){
              git branch: 'main', credentialsId: 'github', url: 'https://github.com/mveyone/devops-.git'
        }
        }
        // stage('Build & push Dockerfile') {
        //     steps {
        //        sh 'ansible-playbook k8s/k8s-playbook.yml'
        //     }
        // }
    } 