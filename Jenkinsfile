pipeline {

  agent none

 stages {
  stage('Unit Test'){
    agent {
	label 'apache'
	}
    steps {
	sh 'ant -f test.xml -v'
	junit 'reports/result.xml'
    }

  }	
  stage("Build"){
     agent {
	label 'apache'
     }
     steps {
       sh 'echo "Building the Java Code"'
       sh 'ant -f build.xml -v'
    }
     post {
       success{
         archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
       }
     }
  }
  stage("Deploy"){
	agent {
	 label 'apache'
	}
	steps {
	 	
	 sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}"

	}
  } 
  stage("Testing on CentOS"){
	agent {
	 label 'CentOS'
	}
	steps {
	 sh "wget http://luckypavan1.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar"
	 sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
	}

  }
  stage("Testing on Debian"){
        agent {
         docker 'openjdk:8u121-jre'
        }
        steps {
         sh "wget http://luckypavan1.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar"
         sh "java -jar rectangle_${env.BUILD_NUMBER}.jar 12 14"
        }

  }
  stage("Promote to Green") {
	
	agent {
	 label 'apache'	
	}
	when{
	  branch 'master'
	}
	steps{
	 	sh "if ![ -d '/var/www/html/rectangles/all/${env.BRANCH_NAME}' ]; then mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME}; fi"
		 sh "cp /var/www/html/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.BUILD_NUMBER}.jar"
	  }


  }
  stage("Promote Development Branch to Master"){
	agent {
         label 'apache'
        }
        when{
          branch 'development'
        }
	steps{
 	  echo "Stashing Any Local Changes"
	  sh 'git stash'
	  echo "Checking Out Development Branch"
	  sh 'git checkout development'
	  echo "Checking Out Master Branch"
	  sh 'git checkout master'
	  echo "Merging Development into Master Branch"
	  sh 'git merge development'
	  echo "Pushing to origin master"
	  sh 'git push origin master'

	}

  }
 		
 }
}



