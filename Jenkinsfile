@Library('Jenkins-Shared-Library')

pipeline {
    agent any
    
    environment {
        dockerHubCredentialsID = 'Dockerhub'                              // DockerHub credentials ID.
        imageName = 'mohamedmasry/nti'                                    // DockerHub repo/image name.
        k8sCredentialsID = 'kubernetes'                                    // KubeConfig credentials ID.
    }
    
    stages {       
        stage('Build Docker image from Dockerfile in GitHub') {
            steps {
                script {
                  buildDockerImage("${imageName}")
                }
            }
        }
        stage('Push image to Docker hub') {
            steps {
                script {
                  pushDockerImage("${dockerHubCredentialsID}", "${imageName}")
                }
            }
        }

        stage('Edit new image in deployment.yaml file') {
            steps {
                script { 
                     {
				        editNewImage("${imageName}")
			        }
                }
            }
        }
        stage('Deploy on k8s Cluster') {
            steps {
                script { 
                     {
				        deployOnKubernetes("${k8sCredentialsID}")
			        }
                }
            }
        }
    }

    post {
        always {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline always succeeded"
        }
        success {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline succeeded"
        }
        failure {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline failed"
        }
    }
}
