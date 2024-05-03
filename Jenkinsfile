pipeline{
    agent any
        tools {
            jdk 'jdk17'
            nodejs 'node16'
            maven 'maven3'
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
       /* stage("Sonar Quality Check"){

            agent{
                docker{
                    image 'maven'
                }
            }
            steps{

                script{

                    withSonarQubeEnv(credentialsId: 'Sonar-Cred')
                     {

                            sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
*/
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
    stage("Sonarqube Analysis") {
            steps {
            
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.url=http://localhost:9000 -Dsonar.login=squ_a9117edf8bb86c79d126c1c393c20edabfcd782d -Dsonar.projectName=hamzademo/cms_synergy_web \
                   -Dsonar.sources=. \
                   -Dsonar.projectKey=hamzademo/cms_synergy_web '''
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