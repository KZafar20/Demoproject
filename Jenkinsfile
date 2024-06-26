pipeline{
    agent any
        tools {
            jdk 'jdk11'
            maven 'maven3'
            nodejs 'node22'
    }
    environment{
                DOCKERHUB_USERNAME = "hamzademo"
                APP_NAME = "cms_synergy"
                IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}" + "_" + "web"
                IMAGE_TAG="latest"
                REGISTRY_CRED = 'Docker-Hub-Token' 
                SCANNER_HOME = tool 'sonar-scanner'
                
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
    stage('Static code analysis'){
            steps{
                
                script{
                    withSonarQubeEnv('Sonar-Server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=CmsSynergyWeb \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=CmsSynergyWeb'''
                   }
                }  
            }
          }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
          }
        }
        stage('Build Docker images') {
                            steps {
                                script   {
                                Docker_Image = docker.build "${IMAGE_NAME}"                           
                            }
                     }
            }
        stage('Push Docker images') {
                            steps {
                                script   {
                                docker.withRegistry('',REGISTRY_CRED){
                                Docker_Image.push(IMAGE_TAG)
                                echo "Deployed Successfully"
                                sh 'docker rmi $IMAGE_NAME:$IMAGE_TAG'
                            }
                         }
                    }
                }
    } 
    post {
        always {
            sh 'docker logout'
        }
    }
}


