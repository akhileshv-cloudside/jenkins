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
    }
}
