pipeline {
    agent any
    environment {
        tag = sh(returnStdout: true, script: "git rev-parse --short HEAD")
    }
    stages {
        stage('Connect gke') {
            steps {
                sh 'gcloud container clusters get-credentials cluster-2 --zone us-central1-c --project akhilesh123'
            }
        }
        stage('Build Image') {
            steps {
                git branch: 'main', url: 'https://github.com/akhileshv-cloudside/jenkins.git'
                sh 'sudo docker build -t us-docker.pkg.dev/akhilesh123/nginx/jenkins/img:$tag'
            }
        }
        stage('Push Image') {
            steps {
               sh 'gcloud auth configure-docker us-docker.pkg.dev -q'
               sh 'sudo docker images'
               sh 'sudo docker push us-docker.pkg.dev/akhilesh123/nginx/jenkins/img:$tag'
            }
        }
        stage('Image remove'){
            steps{
                sh 'sudo docker rmi us-docker.pkg.dev/akhilesh123/nginx/jenkins/img:$tag'
            }
        }
        stage("Set up Kustomize"){
            steps{
                sh 'curl -sfLo kustomize https://github.com/kubernetes-sigs/kustomize/releases/download/v3.1.0/kustomize_3.1.0_linux_amd64'
                sh 'chmod u+x ./kustomize'
            }
        }
        stage("Deploy Image to GKE cluster"){
            steps{
                sh 'kubectl get nodes'
                sh './kustomize edit set image us-docker.pkg.dev/akhilesh123/nginx/jenkins/img:$tag'
                sh './kustomize build . | kubectl apply -f -'
                sleep 60
                sh 'kubectl get services -o wide'
            }
        }                
    }
