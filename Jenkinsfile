node{
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
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.90.121 docker image tag $JOB_NAME:v1.$BUILD_ID mehdibrahim/$JOB_NAME:v1.$BUILD_ID'
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.90.121 docker image tag $JOB_NAME:v1.$BUILD_ID mehdibrahim/$JOB_NAME:latest'
        }
    }
    stage('Push image to Docker hub Hub'){
        sshagent(['ansible_demo']) {
            withCredentials([string(credentialsId: 'dockerhub-passwd', variable: 'dockerhub_passwd')]) {
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.90.121 docker login -u mehdibrahim -p ${dockerhub_passwd}'
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.90.121 docker image push mehdibrahim/$JOB_NAME:v1.$BUILD_ID'
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.90.121 docker image push mehdibrahim/$JOB_NAME:latest'
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.90.121 docker image rm mehdibrahim/$JOB_NAME:v1.$BUILD_ID mehdibrahim/$JOB_NAME:latest'  
               
            }
        }
    }
    stage('Copy files frome ansible to kubernetes'){
        sshagent(['kubernetes_server']) {
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.10.21 cd /home/ubuntu/'
                sh 'scp /var/lib/jenkins/workspace/pipeline-demo/* ubuntu@172.31.10.21:/home/ubuntu/'
       
        }
    }
    stage('kubernetes deployment using ansible'){
        sshagent(['kubernetes_server']) {
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.90.121 cd /home/ubuntu/'
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.90.121 ansible-playbook ansible.yml'
       
        }
    }
       
}