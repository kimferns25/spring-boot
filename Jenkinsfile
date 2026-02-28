pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK'
    }

    environment {
        TOMCAT_HOME = "C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Application') {
            steps {
                dir('prac9') {
                    bat 'mvn clean package'
                }
            }
        }

        stage('Run Tests') {
            steps {
                dir('prac9') {
                    bat 'mvn test'
                }
            }
        }

        stage('Stop Tomcat') {
            steps {
                bat '"%TOMCAT_HOME%\\bin\\shutdown.bat"'
                bat 'ping 127.0.0.1 -n 6 > nul'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                dir('prac9') {
                    bat '''
                    del "%TOMCAT_HOME%\\webapps\\prac9-1.0-SNAPSHOT.war" 2>nul
                    copy target\\*.war "%TOMCAT_HOME%\\webapps\\"
                    '''
                }
            }
        }

        stage('Start Tomcat') {
            steps {
                bat '"%TOMCAT_HOME%\\bin\\startup.bat"'
            }
        }
    }

    post {
        success {
            echo "Application Deployed Successfully!"
        }
        failure {
            echo "Build Failed!"
        }
    }
}
