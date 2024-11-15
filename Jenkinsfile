pipeline {

  environment {
    registry = "docker.io/gopitechskills/flask"
    registry_mysql = "docker.io/gopitechskills/mysql"
    dockerImage = ""
  }

  agent any
    stages {

    stage('Check Docker on Jenkins') {
      steps {
        script {
          try {
            sh 'docker --version'
            echo "Docker is installed on Jenkins server."
          } catch (Exception e) {
            echo "Docker is not installed on Jenkins server."
            error("Docker installation is required on Jenkins server.")
          }
        }
      }
    }
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/gopitechskills/Docker-Project.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('current') {
      steps{
        dir("${env.WORKSPACE}/mysql"){
          sh "pwd"
          }
      }
   }
   stage('Build mysql image') {
     steps{
       sh 'docker build -t "docker.io/gopitechskills/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'docker push "docker.io/gopitechskills/mysql:$BUILD_NUMBER"'
        }
      }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kube")
        }
      }
    }

  }

}
