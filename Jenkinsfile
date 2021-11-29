pipeline {
  environment {
    registry = "sreelekhaa/petclinic"
    registryCredential = 'docker-hub'
    dockerImage = ''
  }
  agent any
  tools {
    maven 'Maven 3.3.9'
    jdk 'jdk8'
  } 
  stages {
    stage('Maven Build Artifact') {
      steps {
        sh '''
        mvn clean install
        ls -ltr target/
        '''
      }
    }
    stage('Building Image') {
      steps{
        script {
          dockerImage = docker.build registry + ":latest"
        }
      }
    }
    stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:latest"
      }
    }
  }
}
