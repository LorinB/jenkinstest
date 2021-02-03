
pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/LorinB/jenkinstest.git', branch:'main'
      }
    }
    
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
      script {
        sh "docker build ./Dockerfile"
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