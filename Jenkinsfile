pipeline {

  environment {
    registryCredential = 'dockerhub1'
  }
  
  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/pegel92/pipelinetest.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("fluppirt/pipelinetest:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "38867ec9-7af8-4514-91c4-01f3965a48e2")
        }
      }
    }

  }

}
