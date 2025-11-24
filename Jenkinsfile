pipeline {
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
                        credentialsId: 'GitHub_Cred'   // <-- ADD THIS
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
                bat '''mvn clean verify sonar:sonar \
                -Dsonar.projectKey=petclinic \
                -Dsonar.projectName=petclinic \
                -Dsonar.host.url=http://localhost:9000 \
                -Dsonar.token=sqp_a0bc0c5a22517db3fb14c441bb9577a3f29fad76'''
            }
        }
    }
}
