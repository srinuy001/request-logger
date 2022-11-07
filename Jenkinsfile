pipeline {
  agent any

  tools {
    maven 'mvn'
  }

  stages {
    stage('Build App') {
      steps {
        sh 'mvn package'
      }
    }
    
    stage('Build Image') {
      steps {
      sh "docker build -t sreenu/request-logger:${env.BUILD_ID} ."
      sh "docker tag sreenu/request-logger:${env.BUILD_ID} sreenu/request-logger:latest"
      }
    }
    
  }

  post {
    always {
      archive 'target/**/*.jar'
    }
    success {
      withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        sh "docker login -u ${USERNAME} -p ${PASSWORD}"
        sh "docker push sreenu/request-logger:${env.BUILD_ID}"
        sh "docker push sreenurequest-logger:latest"
      }
    }
  }
}
