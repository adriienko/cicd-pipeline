pipeline {
  agent any
  stages {
    stage('GitCheckout') {
      steps {
        script {
          checkout scm
        }

      }
    }

    stage('BuildApp') {
      steps {
        sh 'script scripts/build.sh'
      }
    }

    stage('Test') {
      steps {
        script {
          scripts/test.sh
        }

      }
    }

    stage('BuildImage') {
      steps {
        script {
          docker.build("${registry}:${env.BUILD_ID}")
        }

      }
    }

    stage('PushImage') {
      steps {
        script {
          docker.withRegistry('', 'dockerhub-id') {
            docker.image("${registry}:${env.BUILD_ID}").push('latest')
            docker.image("${registry}:${env.BUILD_ID}").push("${env.BUILD_ID}")
          }
        }

      }
    }

  }
  environment {
    registry = 'bogdanandriienko/cicd-pipeline'
  }
}