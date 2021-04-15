pipeline {
    agent any
    stages {
       stage("Parallel Execution") {
			steps {
				parallel(
				      a: {
					        bat "mvn clean"
				      },
				      b: {
					        bat "mvn test"
				      },
				      c: {
					        bat "mvn package"
				      }
				)
			}
		}
	    
	stage('sonar-scanner') {
	      def sonarqubeScannerHome = tool name: 'sonar', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
	      withCredentials([string(credentialsId: 'sonar', variable: 'sonarLogin')]) {
		sh "${sonarqubeScannerHome}/bin/sonar-scanner -e -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=${sonarLogin} -Dsonar.projectName=gs-gradle -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=GS -Dsonar.sources=complete/src/main/ -Dsonar.tests=complete/src/test/ -Dsonar.language=java -Dsonar.java.binaries=."
	      }
    	}    
	    
        stage('Consolidate Results'){
            steps{
                input("Do you capture results?")
                junit '**/target/surefire-reports/TEST-*.xml'
                archive 'target/*.jar'
            }
        }
        stage('Email Notification'){
            steps{
                mail body: "${env.JOB_NAME}  - Build # ${env.BUILD_NUMBER}  - ${currentBuild.currentResult} \n\nCheck console output at ${env.BUILD_URL} to view the results.", subject: "${env.JOB_NAME}  - Build # ${env.BUILD_NUMBER}  - ${currentBuild.currentResult}!!", to: 'pedro.m.onunes@gmail.com'
            }    
        }
    }
}
