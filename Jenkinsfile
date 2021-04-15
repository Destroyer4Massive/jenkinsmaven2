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
