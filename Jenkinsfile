
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
    env:
    - name: DOCKER_HOST
      value: tcp://localhost:2375
  - name: dind
    image: docker:18.05-dind
    securityContext:
      privileged: true
    volumeMounts:
      - name: dind-storage
        mountPath: /var/lib/docker
  volumes:
    - name: dind-storage
      emptyDir: {}
"""
    }
  }
steps {
      
      git url:'https://github.com/LorinB/jenkinstest.git', branch:'main'
      
      script {
        sh "docker build ./"
    } //container
  } //steps
} //stage(build)
    
      stage("Push image") {
            steps {
              container ('builder') {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
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