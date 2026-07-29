pipeline {
    agent any

    environment {
        MAVEN_BIN = 'C:\\Program Files\\apache-maven-3.9.16\\bin'
        TOMCAT_HOME = 'C:\Users\Anshita Srivastava\Downloads\apache-tomcat-9.0.120\apache-tomcat-9.0.120'
        APP_NAME = 'CICD'
    }

    stages {
        stage('Build and test') {
            steps {
                bat '"%MAVEN_BIN%" clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                bat '''
                    call "%TOMCAT_HOME%\\bin\\shutdown.bat"
                    timeout /t 5 /nobreak > nul

                    if exist "%TOMCAT_HOME%\\webapps\\%APP_NAME%" rmdir /s /q "%TOMCAT_HOME%\\webapps\\%APP_NAME%"
                    copy /y "target\\CICD-1.0-SNAPSHOT.war" "%TOMCAT_HOME%\\webapps\\%APP_NAME%.war"

                    call "%TOMCAT_HOME%\\bin\\startup.bat"
                '''
            }
        }
    }

    post {
        always {
            junit testResults: 'target/surefire-reports/*.xml', allowEmptyResults: true
        }
    }
}