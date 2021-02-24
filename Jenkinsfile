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
        stage('run') {
            steps {
                dir('/var/devops/ejemplo-maven') {
                    echo 'run step'
                    sh "nohup bash mvnw spring-boot:run &"
                }
            }
        }
        stage('testing app') {
            steps {
                dir('/var/devops/ejemplo-maven') {
                    sleep 15
                    echo 'testing app step'
                    sh "curl -X GET 'http://localhost:8081/rest/mscovid/test?msg=testing'"
                }
            }
        }
    }
}
