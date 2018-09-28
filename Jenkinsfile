pipeline {
    agent any

	   parameters {
        string(defaultValue: "sometest@example.com",
                description: 'List of email recipients',
                name: 'EMAIL_RECIPIENTS')
		}	
		
    stages {
			stage('Build') {
			  parallel {
			    stage('Build On Windows') {
			    agent {
				   label "windows"
				 }
			      steps {
                     bat "run-tests.bat"
                  }
				}
				stage('Build On Linux') {
                    agent {
                        label "linux"
                    }
                  steps {
                    echo 'Building Project..'
				    sh "cd employees && mvn clean install"
				    }
				}
				}
				}
			stage('Test') {
               steps {
                 echo 'Testing..'
				 sh "cd employees && mvn clean test"	
				}
			  post {
			    always {
			    emailext body: 'Jenkins Pipeline Status',
			    compressLog: true,
			    mimeType: 'text/html',
			    subject: "Pipeline Build ${BUILD_NUMBER}",
			    to: "${params.EMAIL_RECIPIENTS}"
				}
			  }
			}
}
}
