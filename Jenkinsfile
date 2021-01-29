pipeline {
  agent {
    kubernetes {
      label 'spring-petclinic-demo'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: jenkins
  containers:
  - "default-fd5jz"
    env:
    - name: "JENKINS_SECRET"
      value: "********"
    - name: "JENKINS_TUNNEL"
      value: "jenkins-agent:50000"
    - name: "JENKINS_AGENT_NAME"
      value: "default-fd5jz"
    - name: "JENKINS_NAME"
      value: "default-fd5jz"
    - name: "JENKINS_AGENT_WORKDIR"
      value: "/home/jenkins"
    - name: "JENKINS_URL"
      value: "http://jenkins.jenkins.svc.cluster.local:8080/"
    image: "jenkins/inbound-agent:4.6-1"
    imagePullPolicy: "IfNotPresent"
    name: "jnlp"
    resources:
      limits:
        memory: "512Mi"
        cpu: "512m"
      requests:
        memory: "512Mi"
        cpu: "512m"
    tty: false
    volumeMounts:
    - mountPath: "/root/.m2"
      name: "workspace-volume"
      readOnly: false
    workingDir: "/root/.m2"
  nodeSelector:
    kubernetes.io/os: "linux"
  restartPolicy: "Never"
  serviceAccount: "default"
  volumes:
  - emptyDir:
      medium: ""
    name: "workspace-volume"
  - name: docker
    image: docker:latest
    command:
    - cat
    tty: true
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-sock
  volumes:
    - name: docker-sock
      hostPath:
        path: /var/run/docker.sock
    - name: m2
      persistentVolumeClaim:
        claimName: m2
"""
}
   }
  stages {
    
    stage('Check Repo') {
      steps {
        container('default-fd5jz') {
          sh """
             git clone https://github.com/LorinB/jenkinstest.git
          """
        }
      }
    }
    stage('Build') {
      steps {
        container('docker') {
          sh """
             docker build -t LorinB/jenkinstest:$BUILD_NUMBER .
          """
        }
      }
    }
  }
}