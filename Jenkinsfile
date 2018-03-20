node {
	
	stage('CleanUp Workspace'){
	       deleteDir()
	}
	
	stage('Checkout') {
	              git 'https://github.com/kartiksagar14/JavaPipeline.git'
	       }
	
	stage('SonarQube Analysis') {
	
	              withSonarQubeEnv('Sonarqube') {
	              bat "C:/Users/130736/Downloads/sonar-scanner-3.0.3.778-windows/bin/sonar-scanner"
	              }
	       }
	
	 stage('UnitTest') {
	     dir('abc'){
	       try  {
	                     bat 'mvn test'
	       }
	         
	     
	        catch(exc) {
	       stage('JiraBugLog'){
	       withEnv(['JIRA_SITE=LOCAL']){
	              def testIssue = [fields: [ project: [key: 'KS'],
	                           summary: 'New JIRA Created from Jenkins.',
	                           description: 'New JIRA Created from Jenkins.',
	                          issuetype: [name: 'Bug']]]
	
	                     response = jiraNewIssue issue: testIssue
	                     echo response.successful.toString()
	                     echo response.data.toString()
	                    }    
	              }
	         error('Unit Testing Failed so stopping pipeline')
	       }
	     }
	       stage('Build'){
	           dir('abc'){
	        bat 'mvn clean install'
	           }
	  }
	  } 
	}

