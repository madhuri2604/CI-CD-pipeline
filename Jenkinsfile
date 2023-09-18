pipeline{
    agent any
    environment {
        GOOGLE_CLOUD_KEYFILE_JSON = credentials('CI') 
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
                withCredentials([file(credentialsId: 'CI', variable: 'GOOGLE_CLOUD_KEYFILE_JSON')]) {
                    dir("docker-backend"){
                        sh """
                         "gcloud auth activate-service-account --key-file=$GOOGLE_CLOUD_KEYFILE_JSON"
                         "gcloud config set project "$GOOGLE_CLOUD_PROJECT_ID"
                         "sudo docker build -t us-central1-docker.pkg.dev/new-project-399404/my-repo/app-backend:v1 . "
                         "sudo docker push us-central1-docker.pkg.dev/new-project-399404/my-repo/app-backend:v1"
                        """ 
                    }
                }
                            
            }
        }

    }
}
