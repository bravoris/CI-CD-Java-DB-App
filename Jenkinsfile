pipeline {
    agent any

    stages {
        stage("Installing Maven and building app") {
            steps {
                script {
                    sh '''
                        sudo apt update
                        wget -P /tmp https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz
                        sudo tar xf /tmp/apache-maven-3.8.8-bin.tar.gz -C /opt
                        sudo ln -s /opt/apache-maven-3.8.8 /opt/maven
                        echo 'export PATH=$PATH:/opt/maven/bin' >> ~/.bashrc
                        . ~/.bashrc
                        mvn --version
                        rm /tmp/apache-maven-3.8.8-bin.tar.gz
                    '''
                }
            }
        }

        stage("Cloning Project Git Repo and Building") {
            steps {
                script {
                    dir("project") {
                        sh "git clone https://github.com/bravoris/spring-petclinic-project-testing.git"
                        sh "cd spring-petclinic-project-testing && mvn clean install"
                    }
                }
            }
        }

        stage("Building Docker Image") {
            steps {
                script {
                    sh "docker build -t petclinic ."
                    sh "docker run -d -p 8081:8081 petclinic"
                }
            }
        }

        /*
        Uncomment the following stages if you have the appropriate credentials and configurations.

        stage("Logging into JFrog") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'your-jfrog-credentials-id', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh "docker login -u ${USERNAME} -p ${PASSWORD} ${JFROG_URL}"
                }
            }
        }

        stage("Pushing JAR file to JFrog") {
            steps {
                sh "curl -u ${JFROG_USERNAME}:${JFROG_PASSWORD} -T target/your-artifact.jar ${JFROG_URL}/${JFROG_REPO}/"
            }
        }

        stage("Pushing Docker image to JFrog") {
            steps {
                script {
                    docker tag ${DOCKER_IMAGE_NAME} ${JFROG_URL}/${JFROG_REPO}/${DOCKER_IMAGE_NAME}
                    docker push ${JFROG_URL}/${JFROG_REPO}/${DOCKER_IMAGE_NAME}
                }
            }
        }
        */
    }
}
