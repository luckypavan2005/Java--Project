pipeline {
  agent any
  stages {
    stage('Initialize') {
      steps {
        echo 'Hello World of Blue Ocean'
      }
    }
    stage('Build') {
      steps {
        echo 'Started Building'
      }
    }
    stage('Test') {
      steps {
        parallel(
          "Test": {
            echo 'Testing'
            echo 'Junit Testing'
            
          },
          "Functional Testing": {
            echo 'Functional Testing Started'
            
          },
          "Integration Testing": {
            echo 'Integration testing started'
            
          }
        )
      }
    }
    stage('Deploy') {
      steps {
        echo 'Deployment started'
      }
    }
  }
}