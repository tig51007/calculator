pipeline {
    environment {
   	registry ="tig51007/calculator"
	resistryCredentail="dockerhub"
	dockerImage=''
	} 
    agent any
    stages {
        stage ('Check out'){
            steps {
		git bransh: 'uar', url: 'https://github.com/tig51007/calculator.git'
            }
        }
        stage ('Compile') {
            steps {
                sh './gradlew compileJava'
            }
        }
        stage ('Unit Test') {
           steps {
                sh "./gradlew test"
            }
        }
        stage ('Code Coverage'){
            steps {
                sh "./gradlew jacocoTestReport"
                publishHTML ( target: [
                    reportDir: 'build/reports/jacoco/test/html',
                    reportFiles: 'index.html',
                    reportName: "JaCoCo Report"
                ])
                sh "./gradlew jacocoTestCoverageVerification"
            }
        }
        stage ('Static Code Analysis'){
            steps {
                sh "./gradlew checkstyleMain"
                publishHTML (target: [
                    reportDir: 'build/reports/checkstyle/',
                    reportFiles: 'main.html',
                    reportName: "Checkstyle Report"
                ])
            }
        }
	stage('Packaging') {
	    steps {
		sh "./gradlew build"
	    }
        }
	stage('Deploy Image'){
	    steps {
		script {
		   dockerImage = docker.build registry + ":$BUILD_NUMBER"
		}
	    }
        }
    }
}
