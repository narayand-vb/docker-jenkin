pipeline{
    agent any
    
    // setting environment variables
    environment {
        dockerImage = ''
        registry = 'narayandvb/firstreactapp'
        registryCredential = "dockerCred"
    }
    
    // Geting data from git
    stages{
    stage('Test and compose'){
        steps{
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/narayand-vb/docker-jenkin']]])
        }
    }

    // Building image and pushing the image to docker hub
    stage('Building and upload image') {
      steps{
        script {
          dockerImage = docker.build registry
           docker.withRegistry( '', registryCredential ) {
           dockerImage.push()
          }
        }
      }
    }

  // Running the docker image
    stage('deploy') {
      steps {
        bat 'docker ps -f name=reactfirstcontainer'
        bat 'docker container ls -a -fname=reactfirstcontainer '
          script {
            dockerImage.run("-p 3000:3000 --rm --name reactfirstcontainer")
          }
      }
    }
  }
}
