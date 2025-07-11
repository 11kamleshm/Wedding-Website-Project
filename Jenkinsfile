pipeline {
    agent any
    environment {
        KUBECONFIG = "/var/lib/jenkins/.kube/config"
                }
    stages {
        stage("Clone") {
            steps {
                git branch: 'main', url: "https://github.com/11kamleshm/Wedding-Website-Project.git"
            }
        }

        stage("Build Docker Image") {
            steps {
                sh 'docker build -t kamlesh021/website:v1 .'
            }
        }

        stage("Push to Dockerfile") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh """
                        docker login -u $USER -p $PASS
                        docker push kamlesh021/website:v1 
                    """
                }
            }
        }

        stage("Deploy to Kubernetes") {
            steps {                
                sh 'kubectl apply -f website-deply.yaml'
                sh 'kubectl apply -f website-service.yaml'
        }
    }
}
}
