pipeline {
    agent any
 
    environment {
        GOOGLE_APPLICATION_CREDENTIALS = credentials('new-project')
        PROJECT_ID = 'new-project-399404'
        REGION = 'us-central1' 
        IMAGE_NAME = 'my-repo/app-backend'
        IMAGE_TAG = 'v2'
    }
 
    stages {
        stage("clear") {
            steps {
                deleteDir()
            }
        }
        
        stage("git clone") {
            steps {
                sh "git clone https://github.com/madhuri2604/docker-backend.git"
            }
        }
        
        stage('Build and Push Docker Image') {
            steps {
                dir("docker-backend"){
                    script {
                      sh '''  
                         "sudo gcloud auth activate-service-account --key-file= $GOOGLE_APPLICATION_CREDENTIALS "
                         "sudo gcloud config set project $PROJECT_ID"
                         "sudo gcloud auth configure-docker $REGION-docker.pkg.dev --quiet"
                         "sudo docker build -t $REGION-docker.pkg.dev/$PROJECT_ID/$IMAGE_NAME:$IMAGE_TAG ."
                         "sudo docker push $REGION-docker.pkg.dev/$PROJECT_ID/$IMAGE_NAME:$IMAGE_TAG"
                      '''
                    }
                }
            }
        }
    }

}
