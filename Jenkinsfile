pipeline {
    agent any 
		stages 
		{
		    stage('Git checkout') 
             { 
              git 'https://github.com/Lakshmidevy/INGFavBank' 
             }
            stage('Build Analysis') 
             { 
               withSonarQubeEnv('sonar')  
                  { 
                  sh '/opt/maven/bin/mvn clean verify sonar:sonar -Dsonar.password=admin -Dsonar.login=admin -Dmaven.test.failure.ignore=true'
                  } 
             } 
         
            stage("Quality Gate") 
            {
              steps 
			         {
                 timeout(time: 5, unit: 'MINUTES') {
                 waitForQualityGate abortPipeline: true
                     }
                     }
            }
            stage('Deploy') 
             { 
               sh '/opt/maven/bin/mvn clean deploy ' 
             } 
			stage('Execute') 
			 { 
             sh 'export JENKINS_NODE_COOKIE=dontKillMe ;nohup java -jar $WORKSPACE/target/*.jar &' 
             } 
			 
		}
}		
