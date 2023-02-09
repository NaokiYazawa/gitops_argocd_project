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
    stage('Delete Docker Image'){
      steps{
        script{
          sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
        }
      }
    }
    stage('Update kubernetes deployment file'){
      steps{
        script{
          sh """
          cat deployment.yml
          sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
          cat deployment.yml
          """
        }
      }
    }
    stage('Push the changes deployment file to Git'){
      steps{
        script{
          sh """
            git config --global user.name "NaokiYazawa"
            git config --global user.email "yazawa.naoki.01@gmail.com"
            git add deployment.yml
            git commit -m "updated the deployment file"
          """
          withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
            sh "git push https://github.com/NaokiYazawa/gitops_argocd_project.git main"
          }
        }
      }
    }
  }
}
