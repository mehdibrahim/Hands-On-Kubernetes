/*node{
    stage('git checkout'){
        git 'https://github.com/mehdibrahim/Kubernetes_Project.git'
    }
    
    stage('sending docker file to ansible server over ssh'){
        sshagent(['ansible_demo']) {
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.90.121'
                sh 'scp /var/lib/jenkins/workspace/pipeline-demo/* ubuntu@172.31.90.121:/home/ubuntu/'
            }
       }
    stage('Docker Buid image'){
        sshagent(['ansible_demo']) {
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.90.121 cd /home/ubuntu/'
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.90.121 docker image build -t $JOB_NAME:v1.$BUILD_ID .'
        }
    }
     stage('Docker image tagging'){
        sshagent(['ansible_demo']) {
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.90.121 cd /home/ubuntu/'
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.90.121 docker image tag $JOB_NAME:v1.$BUILD_ID .'
        }
    }
       
}*/