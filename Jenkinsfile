pipeline{
	agent any
	// tools{
	// 	maven "test-maven"
	// }
      stages{
           stage('Checkout'){
	    
               steps{
		 echo 'cloning..'
                 git 'https://github.com/prabhatraghav/star-agile-health-care.git'
              }
          }
          stage('Compile'){
             
              steps{
                  echo 'compiling..'
                  sh 'mvn compile'
	      }
          }
          
          stage('Package'){
		  
              steps{
		  
                  sh 'mvn package'
              }
          }
	     stage('deploy'){
    steps{
        sh 'sudo cp -f /var/lib/jenkins/workspace/project/target/*.war /opt/tomcat/webapps/'
    }
}

          
      }
}
