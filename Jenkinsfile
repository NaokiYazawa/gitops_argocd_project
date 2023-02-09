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
          git branch: 'main', url: 'https://github.com/NaokiYazawa/gitops_argocd_project.git'
        }
      }
    }
  }
}
