pipeline{
    agent any
environment {
		DOCKER_LOGIN_CREDENTIALS=credentials('shruthi117-docker')
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

  stage('deploy') {
    steps {
      sh "docker run -itd -p 80:8080 shruthi117/shruthi:$BUILD_NUMBER"
    }
  }

}

}
