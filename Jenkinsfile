pipeline {
    agent any

    tools {
        jdk 'JDK11'
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
                        mvn clean verify sonar:sonar ^
                        -Dsonar.projectKey=petclinic ^
                        -Dsonar.projectName=petclinic ^
                        -Dsonar.host.url=http://localhost:9000 ^
                        -Dsonar.token=%SONAR_TOKEN%
                        -Dmaven.project.sourceRoots.warningsDisabled=true
                    """
                }
            }
        }
    }
}
