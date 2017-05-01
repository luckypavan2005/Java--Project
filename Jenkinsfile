

pipeline {

  agent any

  options {

    buildDiscarder(logRotator(numToKeepStr: '2', artifactNumToKeepStr: '1'))
}
stages {
  stage('Unit Test'){
    steps {
	sh 'ant -f test.xml -v'
	junit 'reports/result.xml
    }

  }	
  stage("build"){
     steps {
       sh 'echo "Building the Java Code"'
       sh 'ant -f build.xml -v'
    }
  }
}
  post {
   always {
     archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
   }
  }

}
