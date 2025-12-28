pipeline {
    agent any

    tools {
        jdk 'JDK17'
        maven 'Maven'
    }

    stages {

        stage('Git checkout') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/pvaranasi95/Petclinic.git',
                        credentialsId: 'GitHub_Cred'
                    ]]
                )
            }
        }

        stage('Maven test') {
            steps {
                bat "mvn test"
            }
        }

        stage('Maven Package') {
            steps {
                bat "mvn clean install"
            }
        }

        stage('Sonar scan') {
    steps {
        withCredentials([string(credentialsId: 'Sonar', variable: 'SONAR_TOKEN')]) {
            bat """
            mvn -U verify org.sonarsource.scanner.maven:sonar-maven-plugin:3.11.0.3922:sonar ^
             -Dsonar.projectKey=petclinic ^
             -Dsonar.projectName=petclinic ^
             -Dsonar.host.url=http://localhost:9000 ^
             -Dsonar.token=%SONAR_TOKEN%
            """
        }
    }
}

    }
}
