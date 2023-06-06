node{
    stage('git checkout'){
        git branch: 'main', url: 'https://github.com/mehdibrahim/Kubernetes_Project'
    }
    
    stage('sending docker file to ansible server over ssh'){
        sshagent(['ansible_demo']) {
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.30.61'
                sh 'scp /var/lib/jenkins/workspace/pipeline-demo/* ubuntu@172.31.30.61:/home/ubuntu/'
            }
       }
    stage('Docker Buid image'){
        sshagent(['ansible_demo']) {
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.30.61 cd /home/ubuntu/'
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.30.61 docker image build -t mehdibrahim/devops-integration .'
        }
    }
    stage('Docker image tagging'){
        sshagent(['ansible_demo']) {
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.30.61 cd /home/ubuntu/'
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.30.61 docker image tag $JOB_NAME:v1.$BUILD_ID mehdibrahim/$JOB_NAME:v1.$BUILD_ID'
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.30.61 docker image tag $JOB_NAME:v1.$BUILD_ID mehdibrahim/$JOB_NAME:latest'
        }
    }
    stage('Push image to Docker hub Hub'){
        sshagent(['ansible_demo']) {
            withCredentials([string(credentialsId: 'dockerhub-passwd', variable: 'dockerhub_passwd')]) {
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.30.61 docker login -u mehdibrahim -p ${dockerhub_passwd}'
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.30.61 docker image push mehdibrahim/$JOB_NAME:v1.$BUILD_ID'
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.30.61 docker image push mehdibrahim/$JOB_NAME:latest'
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.30.61 docker image rm mehdibrahim/$JOB_NAME:v1.$BUILD_ID mehdibrahim/$JOB_NAME:latest'  
               
            }
        }
    }
    stage('Copy files frome ansible to kubernetes'){
        sshagent(['kubernetes_server']) {
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.71.43 cd /home/ubuntu/'
                sh 'scp /var/lib/jenkins/workspace/pipeline-demo/* ubuntu@172.31.71.43:/home/ubuntu/'
       
        }
    }
    stage('kubernetes deployment using ansible'){
        sshagent(['kubernetes_server']) {
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.30.61 cd /home/ubuntu/'
                sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.30.61 ansible-playbook ansible.yml'
       
        }
    }
       
}