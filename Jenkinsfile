
pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/LorinB/jenkinstest.git', branch:'main'
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