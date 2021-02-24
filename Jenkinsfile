pipeline{
    agent any
    stages {
        stage('dir') {
            steps {
                dir('/var/devops/ejemplo-maven') {
                    sh 'pwd'
                }
            }
        }
        stage('compile') {
            steps {
                dir('/var/devops/ejemplo-maven') {
                    echo 'compile step'
                    sh './mvnw clean compile -e'
                }
            }
        }
        stage('test') {
            steps {
                dir('/var/devops/ejemplo-maven') {
                    echo 'test step'
                    sh "./mvnw clean test -e"
                }
            }
        }
        stage('jar/package') {
            steps {
                dir('/var/devops/ejemplo-maven') {
                    echo 'jar step, empaqueta, package'
                    sh "./mvnw clean package -e"
                }
            }
        }
        stage('SonarQube analysis') {
            steps{
                script {
                    withSonarQubeEnv('sonarqube-server') { // You can override the credential to be used
                        sh './mvnw org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                    }
                }
            }
        }
	
    }
}
