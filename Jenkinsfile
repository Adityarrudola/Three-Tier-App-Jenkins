pipeline {
    agent { label 'jenkins-agent' }

    environment {
        DOCKER_REPO = "adityarrudola/react-app"
        IMAGE_NAME = "${DOCKER_REPO}:${BUILD_NUMBER}"
        RESOURCE_GROUP = "aditya"
        ACI_NAME = "react-app-container"
    }

    
    stages {

        stage('Approve Build') {
            steps { 
                input message: 'Do you want to proceed with the build?', ok: 'Yes, proceed'
            }
        }

        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/adityarudola/react-aci-demo.git', branch: 'main'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    '''
                }
            }
        }

        stage('Build & Push Image') {
            steps {
                sh """
                docker buildx build \
                  --platform linux/amd64 \
                  -t ${IMAGE_NAME} \
                  -t ${DOCKER_REPO}:latest \
                  --push .
                """
            }
        }

        stage('Deploy to Azure Container Instances') {
            steps {
                withCredentials([azureServicePrincipal(
                    credentialsId: 'azure-service-principal',
                    subscriptionIdVariable: 'AZ_SUB',
                    clientIdVariable: 'AZ_CLIENT',
                    clientSecretVariable: 'AZ_SECRET',
                    tenantIdVariable: 'AZ_TENANT'
                )]) {

                    sh """
                    az login --service-principal \
                      -u $AZ_CLIENT \
                      -p $AZ_SECRET \
                      --tenant $AZ_TENANT

                    az account set --subscription $AZ_SUB

                    # Delete old container safely
                    az container delete \
                      --resource-group ${RESOURCE_GROUP} \
                      --name ${ACI_NAME} \
                      --yes || true

                    # Create new container
                    az container create \
                      --resource-group ${RESOURCE_GROUP} \
                      --name ${ACI_NAME} \
                      --image ${IMAGE_NAME} \
                      --dns-name-label react-app-${BUILD_NUMBER} \
                      --ports 80 \
                      --restart-policy Always
                    """
                }
            }
        }
    }
}
