pipeline{
    agent any
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
                withCredentials([file(credentialsId: 'CI', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    dir("docker-backend"){
                        sh "gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS"
                        sh "sudo docker build -t us-central1-docker.pkg.dev/new-project-399404/my-repo/app-backend:v1 . "
                        sh "sudo docker push us-central1-docker.pkg.dev/new-project-399404/my-repo/app-backend:v1"
                    }
                }
                            
            }
        }

    }
}
