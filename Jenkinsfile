pipeline {
    agent any
    tools {
        maven "3.8.4"
    }
    stages {
        stage('Compile') {
            steps {
                script {
                    sh "./mvnw clean compile -e"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh "./mvnw clean test -e"
                }
            }
        }
        stage('Jar') {
            steps {
                script {
                    sh "./mvnw clean package -e"
                }
            }
        }
        stage('Run') {
            steps {
                script {
                    sh "nohup bash mvnw spring-boot:run &"
                }
            }
        }
        stage('Wait') {
            steps {
                echo "Sleep 30 seconds"
                sleep(time: 30, unit: "SECONDS")
            }
        }
        stage('Curl') {
            steps {
                script {
                    sh "curl -X GET 'http://localhost:8081/rest/mscovid/test?msg=testing'"
                }
            }
        }
        stage('SonarQube analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonar-scanner'
                    withSonarQubeEnv('sonar-server'){
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
    }
}