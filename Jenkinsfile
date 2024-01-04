pipeline{
  agent any {
    stages {
        stage ("Installing Maven and building app"){
            script{
                sudo apt update

                # Install OpenJDK (if not already installed)
                # sudo apt install -y default-jdk

                # Download Apache Maven
                wget -P /tmp https://apache.osuosl.org/maven/maven-3/3.8.5/binaries/apache-maven-3.8.5-bin.tar.gz

                # Extract the Maven archive
                sudo tar xf /tmp/apache-maven-3.8.5-bin.tar.gz -C /opt

                # Create a symbolic link to make it easier to reference
                sudo ln -s /opt/apache-maven-3.8.5 /opt/maven

                # Add Maven bin directory to the system PATH
                echo 'export PATH=$PATH:/opt/maven/bin' >> ~/.bashrc
                source ~/.bashrc

                # Verify the installation
                mvn --version

                # Clean up
                rm /tmp/apache-maven-3.8.5-bin.tar.gz
            }
            }
        stage ( "Cloning Project Git Repo an Building"){
            sh " cd "
            sh " mkdir project"
            sh " cd project/"
            sh " git clone https://github.com/bravoris/spring-petclinic-project-testing.git"
            sh " maven build "
            }
        stage ("Building Docker Image")
            {
            script{
                    docker build -t petclinic .
                    docker run -d -p 8081:8081 petclinic
                }
            }
        //stage ("Logging into jfrog"){
           // script{
                //withCredentials([usernamePassword(credentialsId: 'your-jfrog-credentials-id', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        //sh "docker login -u ${USERNAME} -p ${PASSWORD} ${JFROG_URL}"
            }
        }
        }
        //stage ("Pushing jar file to jfrog "){
             //sh "curl -u ${JFROG_USERNAME}:${JFROG_PASSWORD} -T target/your-artifact.jar ${JFROG_URL}/${JFROG_REPO}/"
        }
        //stage ("Pushing Docker image to jfrog"){
            //script{
                //docker tag ${DOCKER_IMAGE_NAME} ${JFROG_URL}/${JFROG_REPO}/${DOCKER_IMAGE_NAME}
                //docker push ${JFROG_URL}/${JFROG_REPO}/${DOCKER_IMAGE_NAME}
            }
        }
        }
    }
  }  
}