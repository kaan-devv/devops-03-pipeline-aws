pipeline {
    agent {
        node {
            label 'My-Jenkins-Agent'
        }
    }
    tools {
        maven 'Maven3'
        jdk 'Java21'
    }

      environment {

            APP_NAME = "devops-03-pipeline-aws"
            RELEASE = "1.0"
            DOCKER_USER = "kaanylmz"
            DOCKER_LOGIN = 'id_dockerhub'
            IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
            IMAGE_TAG = "${RELEASE}.${BUILD_NUMBER}"

          //   kaanylmz/devops-03-pipeline-aws:latest
          //     ${DOCKER_USER}/${APP_NAME}:${IMAGE_TAG}

        }



    stages {
        stage('SCM GitHub') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kaan-devv/devops-03-pipeline-aws']])
            }
        }
        stage('Test Maven') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn test'
                    } else {
                        bat 'mvn test'
                    }
                }
            }
        }
        stage('Build Maven') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn clean install'
                    } else {
                        bat 'mvn clean install'
                    }
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('MySonarQubeServer') {
                        if (isUnix()) {
                            sh 'mvn sonar:sonar'
                        } else {
                            bat 'mvn sonar:sonar'
                        }
                    }
                }
            }
        }
        stage("Quality Gate"){
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                }
            }
        }



        /*

         stage('Docker Image') {
             steps {
             //    sh 'docker build  -t mimaraslan/devops-application:latest   .'
                 bat 'docker build  -t mimaraslan/devops-application:latest   .'
             }
         }


         stage('Docker Image To DockerHub') {
             steps {
                 script {
                     withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {

                         if (isUnix()) {
                              sh 'docker login -u mimaraslan -p %dockerhub%'
                              sh 'docker push mimaraslan/devops-application:latest'
                           } else {
                              bat 'docker login -u mimaraslan -p %dockerhub%'
                              bat 'docker push mimaraslan/devops-application:latest'
                          }
                     }
                 }
             }
         }
         */

        stage('Build & Push Docker Image to DockerHub') {
            steps {
                script {

                    docker.withRegistry('', DOCKER_LOGIN) {

                        docker_image = docker.build "${IMAGE_NAME}"
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push("latest")
                    }
                }
            }
        }

     /*

         stage('Deploy Kubernetes') {
             steps {
             script {
                     kubernetesDeploy (configs: 'deployment-service.yaml', kubeconfigId: 'kubernetes')
                 }
             }
         }


         stage('Docker Image to Clean') {
             steps {

                    //  sh 'docker image prune -f'
                      bat 'docker image prune -f'

             }
         }
         */
    }


}