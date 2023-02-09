pipeline{
  agent any

  environment{
    DOCKERHUB_USERNAME = "farstep131"
    APP_NAME = "gitops-argo-app"
    IMAGE_TAG = "${BUILD_NUMBER}"
    IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
    REGISTRY_CREDS = "dockerhub"
  }

  stages {
    stage('Cleanup workspace'){
      steps{
        script{
          cleanWs()
        }
      }
    }
    stage('Checkout SCM'){
      steps{
        script{
          git branch: 'main',
              credentialsId: '',
              url: 'https://github.com/NaokiYazawa/gitops_argocd_project.git'
        }
      }
    }
    stage('Build Docker Image'){
      steps{
        script{
          docker_image = docker.build "${IMAGE_NAME}"
        }
      }
    }
    stage('Push Docker Image'){
      steps{
        script{
          docker.withRegistry('', REGISTRY_CREDS){
            docker_image.push("${BUILD_NUMBER}")
          }
        }
      }
    }
  }
}
