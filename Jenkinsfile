pipeline {
    agent any
  environment {
    PATH = "/usr/sbin:$PATH"
  }
    stages {
        stage('Helm Package Creation') {
            steps {
                sh '''
                helm create phpmyadmin
                rm -rf ./phpmyadmin/templates/*
                cp *.yaml ./phpmyadmin/templates/
                helm lint phpmyadmin/
                helm template phpmyadmin
                helm package phpmyadmin
                '''
            }
        }
        stage('Deploy to K8s') {
            steps {
                sshagent(['Kubernetes']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no ./*.tgz ubuntu@13.126.220.38:/home/ubuntu
                    scp -o StrictHostKeyChecking=no /home/ubuntu/linux-amd64/helm ubuntu@13.126.220.38:/home/ubuntu
                    ssh -t ubuntu@13.126.220.38 /bin/bash << 'EOF' 
                    sudo mv ./helm /usr/local/bin/ 
                    sudo chmod +x /usr/local/bin/helm 
                    export PATH=/usr/local/bin/:$PATH
                    helm upgrade --install phpmyadm ./*.tgz 
EOF
                     '''
                    }
            }
        }
        
    }
}
