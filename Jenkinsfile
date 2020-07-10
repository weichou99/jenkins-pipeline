pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v $HOME/.m2:/root/.m2'
        }
    }
    stages {
	    stage('Info') {

            steps {
                sh "echo $HOME"
				sh "pwd"
            }
        }
        stage('Build') {

            steps {
                git 'https://github.com/weichou99/autotest.git'
                sh "mvn -f ./code/autotest/pom.xml resources:resources compiler:compile"
            }
        }
        stage('Test') {
            steps {
                sh "mvn -f ./code/autotest/pom.xml -Dmaven.test.failure.ignore=true resources:testResources compiler:testCompile surefire:test"
            }

        }
        stage('Package') {
            steps {
                sh "mvn -f ./code/autotest/pom.xml jar:jar"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts '**/target/*.jar'
                }
            }
        }
    }
}
