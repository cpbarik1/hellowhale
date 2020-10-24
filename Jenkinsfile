pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/cpbarik1/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                   myapp = docker.build("cpbarik1/hellowhale:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub1') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "hellowhaledeploy.yml", kubeconfigId: "mykubeconfig1")
        }
      }
    }

  }

}
