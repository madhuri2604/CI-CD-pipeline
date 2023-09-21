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
        stage('Change File'){
          steps{
                dir("helm-chart"){
                    sh '''
cat > values.yaml << EOL
# Default values for mychart.
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

replicaCount: 2

imagePullSecrets: []
nameOverride: ""
fullnameOverride: ""

serviceAccount:
  # Specifies whether a service account should be created
  create: true
  # Annotations to add to the service account
  annotations: {}
  # The name of the service account to use.
  # If not set and create is true, a name is generated using the fullname template
  name: ""

podAnnotations: {}

podSecurityContext: {}
  # fsGroup: 2000

securityContext: {}
  # capabilities:
  #   drop:
  #   - ALL
  # readOnlyRootFilesystem: true
  # runAsNonRoot: true
  # runAsUser: 1000

service:
  port: 80

resources: {}

autoscaling:
  enabled: false
  minReplicas: 1
  maxReplicas: 100
  targetCPUUtilizationPercentage: 80
  # targetMemoryUtilizationPercentage: 80

nodeSelector: {}

tolerations: []

affinity: {}

backend:
  image:
    repository: $REGION-docker.pkg.dev/$PROJECT_ID/$IMAGE_NAME:$IMAGE_TAG
  targetPort: 5000
  servicetype: LoadBalancer

frontend:
  image:
    repository: us-central1-docker.pkg.dev/jenkins-399608/repo/frontend1:v1
  targetPort: 80
  servicetype: LoadBalancer
EOL
                 
'''

                }
            }
        }
        stage('Change'){
          steps{
                dir("helm-chart"){
                    sh "cat values.yaml"
                }
          }
        }
    }
}
