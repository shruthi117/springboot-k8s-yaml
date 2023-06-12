pipeline{
    agent any
tools{
        maven 'maven'
    }
    
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/shruthi117/springboot-k8s-yaml.git']]])
                sh 'mvn clean install'
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
