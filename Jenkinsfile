
pipeline {
  agent {
    label 'master'
  }

  environment {
    APP_NAME = "konakart"
    // file locations
    DOCKER_COMPOSE_TEMPLATE_FILE ="$WORKSPACE/docker/docker-compose.template.yaml"
    DOCKER_COMPOSE_FILE="$WORKSPACE/docker/docker-compose.yaml"
  }
  stages {

   stage('create docker network') {

                              steps {
                                   sh "docker network create ${APP_NAME} || true"

                              }
               }
               stage('configure-docker-compose-file') {
                           steps {
                               script {
                                   echo "============================================="
                                   echo "Deployment configuration"
                                   echo "Docker Network : ${APP_NAME}"
                                   echo "============================================="

                                   // update the docker-compse file with the new image names
                                   sh "cp  '${DOCKER_COMPOSE_TEMPLATE_FILE}' '${DOCKER_COMPOSE_FILE}'"
                                   sh "sed -i 's#TO_REPLACE#${APP_NAME}#g' '${DOCKER_COMPOSE_FILE}'"
                                   sh "cat '${DOCKER_COMPOSE_FILE}'"
                               }
                           }
               }

               stage('docker-down') {
                         steps {
                          sh "docker-compose -f '${DOCKER_COMPOSE_FILE}' down "
                         }
               }

               stage('docker-compose-up') {
                 steps {
                          sh "docker-compose -f '${DOCKER_COMPOSE_FILE}' up -d"
                 }
                }




           }
           post {
                 always {
                         cleanWs()
                         sh 'docker volume prune'
                 }

               }
         }
