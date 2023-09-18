pipeline{
    agent any
    environment {
        GOOGLE_CLOUD_KEYFILE_JSON = credentials('gcp-key') 
        GOOGLE_CLOUD_PROJECT_ID = 'new-project-399404' 
    }
    stages{
        stage("Clean Up"){
            steps {
                deleteDir()
            }
        }
        stage("clone repo"){
            steps {
                sh "git clone https://github.com/madhuri2604/docker-backend.git"
            }
        }
        stage("Build"){
            steps{
                
                dir("docker-backend"){
                    sh """
                     "gcloud auth activate-service-account --key-file=${GOOGLE_CLOUD_KEYFILE_JSON}"
                     "gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project ${GOOGLE_CLOUD_PROJECT_ID}"
                     "sudo docker build -t us-central1-docker.pkg.dev/new-project-399404/my-repo/app-backend:v1 . "
                     "sudo docker push us-central1-docker.pkg.dev/new-project-399404/my-repo/app-backend:v1"
                    """ 
                }
                
                            
            }
        }

    }
}
