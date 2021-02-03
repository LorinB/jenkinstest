
pipeline {

  agent none

  stages {

        
      stage('Build') {
  agent {
    kubernetes {
      label 'jenkinsrun'
      defaultContainer 'builder'
      yaml """
kind: Pod
metadata:
  name: docker-build
spec:
  containers:
  - name: builder
    image: jenkins/jnlp-agent-docker:latest
    imagePullPolicy: Always
    command:
    - cat
    tty: true
"""
    }
  }
steps {
      
      git url:'https://github.com/LorinB/jenkinstest.git', branch:'main'
      
      script {
        sh "docker build jenkinstest"
    } //container
  } //steps
} //stage(build)
    
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

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "billing-postgresql.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}