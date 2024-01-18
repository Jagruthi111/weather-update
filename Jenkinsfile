pipeline {
    agent {
        label 'slave01'
    }

    environment {
        TOMCAT_HOST = '172.31.2.55'
        TOMCAT_USER = 'root'
        TOMCAT_DIR = '/opt/apache-tomcat-8.5.98/webapps'
        JAR_FILE = 'weather-update-1.0-SNAPSHOT.jar'  // Replace with the actual name of your JAR file
    }

    stages {
        stage('checkout') {
            steps {
                sh 'rm -rf '
                sh 'https://github.com/Jagruthi111/weather-update.git'
            }
        }

        stage('build') {
            steps {
                script {
                    def mvnHome = tool 'Maven'
                    def mvnCMD = "${mvnHome}/bin/mvn"
                    sh "${mvnCMD} clean install"
                }
            }
        }


        stage('run jar file') {
            steps {
                script {
                   
                    sh "java -jar target/${JAR_FILE}"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                   
                    sh "scp target/${JAR_FILE} ${TOMCAT_USER}@${TOMCAT_HOST}:${TOMCAT_DIR}/"

                    sh "ssh ${TOMCAT_USER}@${TOMCAT_HOST} 'bash -s' < restart-tomcat.sh"

                }
            }
        }
    }
}
