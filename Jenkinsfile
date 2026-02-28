pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK24'
    }

    environment {
        TOMCAT_HOME = "/Users/kim/Downloads/apache-tomcat-10.1.46"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Application') {
            steps {
                dir('prac9') {   // remove this line if pom.xml is in root
                    sh 'mvn clean package'
                }
            }
        }

        stage('Run Tests') {
            steps {
                dir('prac9') {   // remove if pom.xml is in root
                    sh 'mvn test'
                }
            }
        }

        stage('Stop Tomcat') {
            steps {
                sh '$TOMCAT_HOME/bin/shutdown.sh || true'
                sh 'sleep 5'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                dir('prac9') {   // remove if pom.xml is in root
                    sh 'cp target/*.war $TOMCAT_HOME/webapps/'
                }
            }
        }

        stage('Start Tomcat') {
            steps {
                sh '$TOMCAT_HOME/bin/startup.sh'
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
