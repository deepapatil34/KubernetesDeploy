pipeline {
    agent any
  environment {
    PATH = "/usr/sbin:$PATH"
  }
    stages {
        stage('Helm Package Creation') {
            steps {
                sh "helm create phpmyadmin"
                sh "rm -rf ./phpmyadmin/templates/*"
                sh "cp Kubernetes/*.yaml ./phpmyadmin/templates/"
                sh "helm package phpmyadmin"
                
                
            }
        }
        stage('Deploy to K8s') {
            steps {
                sshagent(['Kubernetes']) {
                    sh "scp -o StrictHostKeyChecking=no ./*.tgz ubuntu@13.235.245.136:/home/ubuntu"
                    
                    sh "helm install phpmyadm ./*.tgz"
}
            }
        }
    }
}
