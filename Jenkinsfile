pipeline{
    agent any

    environment{
                DOCKERHUB_USERNAME = "hamzademo"
                APP_NAME = "cms_synergy"
                IMAGE_TAG = "${BUILD_NUMBER}"
                IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
                REGISTRY_CRED = 'DockerHub'
        }
    stages{

        stage("clean workspace"){

            steps{

                script{

                    cleanWs()
                }
            }
        }
        stage("Checkout from source code"){
            steps{

                script{

                    git branch: 'main', 
                    credentialsId: 'github',
                    url: 'https://github.com/KZafar20/Demoproject.git'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script{

                sh "npm install"
                    }  
          }
        }
    } 
}