
pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/LorinB/jenkinstest.git', branch:'main'
      }
    }
    
    stage("Build image") {
            steps {
                script {
                    myapp = docker.build("postgres/postgres:13.0-alpine")
                }
            }
        }
    
      stage("Push image") {
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