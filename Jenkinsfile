pipeline {
    agent {label 'slave01'}

      environment {
        MAVEN_HOME = tool 'Maven'
        //SPRING_PROFILES_ACTIVE = 'local' 
    }


    stages {
        stage('checkout') {
            steps {
                sh 'rm -rf '
                sh 'git clone https://github.com/Jagruthi111/weather-update.git'
            }
        }

        stage('build') {
            steps {
                script {
                     sh "${MAVEN_HOME}/bin/mvn clean package"
                }
            }
        }


        stage('run jar') {
            steps {
                script {
                   
                    sh "java -jar target/weather-update-1.0-SNAPSHOT.jar&"

                    sleep 30
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
			 sh 'ssh root@172.31.2.55'
			 sh 'scp target/weather-update-1.0-SNAPSHOT.jar root@172.31.2.55:/opt/apache-tomcat-8.5.98/webapps/'

                }
            }
        }
    }
}
