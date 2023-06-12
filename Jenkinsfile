pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = "mohamedbedier/instabugtask"
        DOCKERFILE_PATH = "internship/"
        DOCKER_REPO_CREDENTIALS = "docker-repo"
    }
    stages {
        stage('Build and Pushing Docker Image') {
            steps {
                script {
                    def dockerImageTag = "${env.BUILD_NUMBER}"
                    withCredentials([usernamePassword(credentialsId: env.DOCKER_REPO_CREDENTIALS, passwordVariable: 'DOCKER_REPO_PASSWORD', usernameVariable: 'DOCKER_REPO_USERNAME')]) {
           
                        // Build Docker image and tag it
                        sh "docker build -t ${env.DOCKER_IMAGE_NAME}:${dockerImageTag} ${env.DOCKERFILE_PATH}"                       
                        // Login to Docker repository
                        sh "docker login -u ${env.DOCKER_REPO_USERNAME} -p ${DOCKER_REPO_PASSWORD}"                      
                        // Push Docker image to repository
                        sh "docker push ${env.DOCKER_IMAGE_NAME}:${dockerImageTag}"                   
                        // Remove local Docker image
                        sh "docker rmi ${env.DOCKER_IMAGE_NAME}:${dockerImageTag}"
                    }
                
                }
            }
        }
        stage ('pushing image version to helm repo..') {
                 steps {       
                script { 
                      def chartPath = 'helmchart/instabugchart'
                 withCredentials( [ usernamePassword(credentialsId: 'git-repo' , passwordVariable: 'PASS' , usernameVariable: 'USER' )] ) {
       
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'
                        sh ' rm -rf instabugtask/'
                        sh """
                        git clone  https://$USER:$PASS@github.com/Bedier1/instabugtask.git
                        cd instabugtask/
                        git remote set-url origin https://$USER:$PASS@github.com/Bedier1/instabugtask.git
                        sed -i 's/tag:.*/tag: \"${BUILD_NUMBER}\"/' ${chartPath}/values.yaml
                        git add ${chartPath}/values.yaml
                        git commit -m " adjusting version of the image"
                        git push  -f origin HEAD:main
                            """
                       }
                }
                }  
        }
    }
    post {
        always {
            script {
      def buildStatus = currentBuild.result
                def buildNumber = currentBuild.number
                def buildUrl = "${env.BUILD_URL}"
                def buildCommit = "${env.GIT_COMMIT}"
                if (buildStatus == 'SUCCESS') {
                    slackSend(
                        channel: '#jenkinsintegration', 
                        message: "Build ${buildNumber} succeeded! \n" +
                                 "Build URL: ${buildUrl}"
                    )
                } else {
                    slackSend(
                        channel: '#jenkinsintegration', 
                        message: "Build ${buildNumber} failed! \n" +
                                 "Build URL: ${buildUrl}"
                    )
                }

            }

        }
    }
}
