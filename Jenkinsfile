    node{
        
        stage('git checkout'){
        git branch: 'main', credentialsId: 'github', url: 'https://github.com/mveyone/devops-.git'
        }
        // stage('build docker image'){
        //      steps{
        //       sh 'docker build -t $JOB_NAME:v1.$BUILD_ID . '
           
        //   }
        // }
        // stage('image tagging '){
        //     steps {
        //      sh 'docker image tag $JOB_NAME:v1.$BUILD_ID mveyone/$JOB_NAME:v1.$BUILD_ID '
        //      sh 'docker image tag $JOB_NAME:v1.$BUILD_ID mveyone/$JOB_NAME:latest '
        //   }
        // }
        // stage('Push docker image to docker hub'){
        //     steps{
        //            //sh "ssh -o StrictHostKeyChecking=no ubuntu@10.0.2.81  docker login -u mveyone -p ${dockerhub-pass}"
        //            sh '  docker image push mveyone/$JOB_NAME:v1.$BUILD_ID'
        //            sh '  docker image push mveyone/$JOB_NAME:latest '
        //            sh '  docker image rm mveyone/$JOB_NAME:latest  mveyone/$JOB_NAME:v1.$BUILD_ID $JOB_NAME:v1.$BUILD_ID'
            
        //         }
        // }
        // // stage('deploy to eks k8s'){
        // //     steps{
        // //            sh ' ansible-playbook k8s/k8s-playbook.yml'   
        // //          }
        // // }
    }