pipeline{
  agent any {
    stages {
        stage ( "Cloning Project Git Repo"){
            sh " git clone "
            sh " cd "
            }
        stage ("Installing Maven and building app"){
            script{
                def mvnHome = tool 'Maven'
                    env.PATH = "${mvnHome}/bin:${env.PATH}"
                Maven clean install
            }
            }
        stage ("Building Docker Image")
            {
            script{
                    Docker build -t javaapp/latest .
                }
            }
        stage ("Logging into jfrog"){
            script{
                withCredentials([usernamePassword(credentialsId: 'your-jfrog-credentials-id', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh "docker login -u ${USERNAME} -p ${PASSWORD} ${JFROG_URL}"
            }
        }
        }
        stage ("Pushing jar file to jfrog "){
             sh "curl -u ${JFROG_USERNAME}:${JFROG_PASSWORD} -T target/your-artifact.jar ${JFROG_URL}/${JFROG_REPO}/"
        }
        stage ("Pushing Docker image to jfrog"){
            script{
                docker tag ${DOCKER_IMAGE_NAME} ${JFROG_URL}/${JFROG_REPO}/${DOCKER_IMAGE_NAME}
                docker push ${JFROG_URL}/${JFROG_REPO}/${DOCKER_IMAGE_NAME}
            }
        }
        }
    }
  }  
}