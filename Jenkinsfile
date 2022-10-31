pipeline {
  environment {
    imagename = "khanhnguyen7496/jenkins-docker"
    registryCredential = 'nhk-dockerhub'
    dockerImage = ''
    repoUrl = 'https://github.com/khanhnguyen7496/simple-demo.git'
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/khanhnguyen7496/simple-demo.git', branch: 'main'])
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Running image') {
      steps{
        script {
          sh "docker run ${imagename}:latest"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')
            sh """
            git tag -a ${BUILD_NUMBER} -m "build number ${BUILD_NUMBER}"
            git push ${repoUrl} ${BUILD_NUMBER}
            """
          }
        }
      }
    }
  }
}