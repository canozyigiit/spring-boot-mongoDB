pipeline {
  agent any
  environment {
    IMAGE_REGISTRY = "canozyigiit/spring-jenkinsfile-ex"
    IMAGE_VERSION = 'latest'
    IMAGE_REGISTRY_CREDENTIAL = 'dockerhub'
    DOCKER_REGISTRY_URL = ""
  }
    stages {
       stage('Build') {
            agent {
                docker {
                    image 'maven:3.8.1-adoptopenjdk-11'
                    args '-v $HOME/.m2:/root/.m2'
                    reuseNode true
                }
            }
            steps {
                sh """
                mvn compile jib:dockerBuild
                """
            }
       }

       stage('Push image') {
            steps {
                withDockerRegistry([credentialsId: "${IMAGE_REGISTRY_CREDENTIAL}", url: "https://registry.hub.docker.com"]) {
                    sh "docker login -u 'canozyigiit' -p 'verylongpasswordhere' "
                    sh "docker push ${IMAGE_REGISTRY}:${IMAGE_VERSION}"
                }
            }
       }
    }
  }