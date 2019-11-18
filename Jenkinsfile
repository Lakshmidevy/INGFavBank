pipeline {
    agent any 
		stages 
		{
		stage('Git'){
			steps
             { 
              git 'https://github.com/Lakshmidevy/INGFavBank' 
             }
		    }
        stage('Build'){
			 steps{
             { 
               withSonarQubeEnv('sonar')  
                  { 
                  sh '/opt/maven/bin/mvn clean verify sonar:sonar -Dsonar.password=admin -Dsonar.login=admin -Dmaven.test.failure.ignore=true'
                  } 
             } 
             }
			 }
            stage("Quality Gate"){
             steps 
			        {
                 timeout(time: 5, unit: 'MINUTES') {
                 waitForQualityGate abortPipeline: true
                     }
                     }
            }
            stage('Deploy') 
			 steps{
             { 
               sh '/opt/maven/bin/mvn clean deploy ' 
             } 
			 }
			stage('Execute') 
			 steps{
				{ 
             sh 'export JENKINS_NODE_COOKIE=dontKillMe ;nohup java -jar $WORKSPACE/target/*.jar &' 
             } 
			 }
		}
}		
