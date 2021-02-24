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
                dir('/var/devops/ejemplo-maven') {
                    script {
                        withSonarQubeEnv('sonarqube-server') { // You can override the credential to be used
                            sh './mvnw org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                        }
                    }
                }
            }
        }
       stage('uploadNexus'){
            steps{
                script {
                    nexusPublisher nexusInstanceId: 'nexus', nexusRepositoryId: 'test-nexus', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: 'jar', filePath: '/var/devops/ejemplo-maven/build/DevOpsUsach2020-0.0.1.jar']], mavenCoordinate: [artifactId: 'DevOpsUsach2020', groupId: 'com.devopsusach2020', packaging: 'jar', version: '0.0.1']]], tagName: '0.0.1'
                }
            }
	    }
    }
}
