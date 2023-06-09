pipeline {
    agent any
    environment {
        tag = sh(returnStdout: true, script: "git rev-parse --short HEAD")
    }
    stages {
        stage('Connect gke') {
            steps {
                sh 'gcloud container clusters get-credentials ak-cluster --zone us-central1-c --project akhilesh123'
            }
        }
        stage('Build Image') {
            steps {
                git branch: 'main', url: 'https://github.com/akhileshv-cloudside/jenkins.git'
                sh 'sudo docker build -t us-docker.pkg.dev/akhilesh123/nginx/img:$tag .'
            }
        }
        stage('Push Image') {
            steps {
               sh 'sudo gcloud auth configure-docker us-docker.pkg.dev'
               sh 'sudo docker images'
               sh 'sudo docker push us-docker.pkg.dev/akhilesh123/nginx/img:$tag'
            }
        }
    }
}
