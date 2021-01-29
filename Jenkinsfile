
pipeline {

  agent none

  stages {

    stage('Checkout Source') {
      agent any
      steps {
        git url:'https://github.com/LorinB/jenkinstest.git', branch:'main'
      }
    }
    
    stage("Build image") {
            agent {
              label 'jenkins-docker-slave'
            }
            steps {
                script {
                    myapp = docker.build("LorinB/jenkinstest:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            agent {
              label 'jenkins-docker-slave'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
      
    
    

  }

}