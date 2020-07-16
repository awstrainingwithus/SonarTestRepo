pipeline{
   agent any
   tools {
        maven "mymaven" 
    }
   stages{
   stage('Checkout'){
      
      steps{
	        script{
		            git 'https://github.com/awstrainingwithus/SonarTestRepo.git'
	            }
            echo "Checkout is Completed"
      }
   }
   stage('Compile-Package'){
      steps{
       script{
		 sh "mvn -version"
         sh "mvn package"
		 echo "Maven Compile Stage is completed"
	 }
   }
   }
   
   stage('SonarQube Analysis') {
      steps{
         script{
         
        withSonarQubeEnv('sonar') { 
          sh "mvn sonar:sonar"
	}
        }
        }
    }
   
  stage("Quality Gate Statuc Check"){
	  steps{
          timeout(time: 1, unit: 'HOURS') {
		  script{
			  sleep (120)
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
	      echo "Quality gate failed"
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
		if (qg.status != 'FAILURE') {
	      echo "Quality gate passed"
                  
              }
		  }
	  }
          }
      }    
   
}
}
