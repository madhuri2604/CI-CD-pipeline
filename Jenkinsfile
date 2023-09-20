pipeline {
    agent any

    environment {
        Last_Commit_Id = "${env.GIT_COMMIT}"
        GOOGLE_APPLICATION_CREDENTIALS = credentials('new-jenkins')
        PROJECT_ID = 'jenkins-399608'
        REGION = 'us-central1'  
        IMAGE_NAME = 'repo/backend-1'
        IMAGE_TAG = "${Last_Commit_Id}"
    }

    stages {
        stage("Remove dir") {
            steps {
                deleteDir()
            }
        }

        stage("Clone") {
            steps {
                sh "git clone https://github.com/madhuri2604/docker-backend.git"
                sh "git clone https://github.com/madhuri2604/helm-chart.git"
            }
        }
        
        // stage("approval") {
        //     options {
        //         timeout(time: 1, unit: 'MINUTES')
        //     }
        //     steps {
        //         input message: "Please approve to proceed with deployment", submitter: "your-approver-user"
        //     }
        // }

        stage('Build and Push Docker Image') {
            steps {
                dir("docker-backend") {
                    script {
                        sh '''
                            sudo gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS
                            sudo gcloud config set project $PROJECT_ID
                            sudo gcloud auth configure-docker $REGION-docker.pkg.dev --quiet
                            sudo docker build -t $REGION-docker.pkg.dev/$PROJECT_ID/$IMAGE_NAME:$IMAGE_TAG .
                            sudo docker push $REGION-docker.pkg.dev/$PROJECT_ID/$IMAGE_NAME:$IMAGE_TAG
                        '''
                    }
                }
            }
        }
        stage("deployment") {
            steps {
                dir("helm-chart"){
                    script{
                        sh '''
                            sudo apt-get install kubectl
                            sudo gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project jenkins-399608
                            helm lint
                            helm install uchart demochart
                        '''
                    }
                }
            }
        }
    }
}
