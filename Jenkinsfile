pipeline{
    agent any 
environment {
		DOCKER_LOGIN_CREDENTIALS=credentials('shruthi117-dockerhub')
	}
    stages {
  stage('checkout') {
    steps {
      git 'https://github.com/shruthi117/springboot-k8s-yaml.git'
    }
  }

  stage('build') {
    steps {
      sh 'mvn clean install'
      sh 'docker build -t shruthi117/shruthi:$BUILD_NUMBER .' 

    }
  }

  stage('push') {
    steps {
      sh 'echo $DOCKER_LOGIN_CREDENTIALS_PSW | docker login -u $DOCKER_LOGIN_CREDENTIALS_USR --password-stdin'
      sh 'docker push shruthi117/shruthi:$BUILD_NUMBER'
    }
  }

   stage('Deploy to K8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'kubernetes')
                }
            }
        }

}

}
