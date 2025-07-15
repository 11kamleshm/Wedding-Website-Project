pipeline{
    agent any
    stages{
        stage("CLone"){
            steps{
                git branch: 'main', url:"https://github.com/11kamleshm/Wedding-Website-Project.git"
            }
        }
        stage("Build Docker Image"){
            steps{
                sh 'docker build -t kamlesh021/website:v1 .'
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(credentialsId:'dockerhub-creds',usernameVariable:'User',passwordVariable:'Pass')]){
                    sh """
                    docker login -u $USER -p $PASS
                    docker push kamlesh021/website:v1
                    """
                }
            }
        }
        stage("Deploy to kubernetes"){
            steps{
                sh "kubectl apply -f website-deployment.yaml"
                sh "kubectl apply -f website-service.yaml"
            }
        }
    }
}