pipeline {
    agent {
        node{label 'Windows1'}
    }
    tools {
        jdk 'JDK11'
        maven 'Maven'
    }

    stages {
//         stage('Git checkout') {
//             steps {
//                 checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/pvaranasi95/Petclinic.git']])
//             }
//         }
//         // stage('Maven test') {
//         //     steps{
//         //         bat "mvn test"
//         //     }
//         // }
//         // stage('Maven Package') {
//         //     steps{
//         //         bat "mvn clean install"
//         //     }
//         // }
//         stage('Sonar scan') {
//             steps{
//                 bat '''mvn clean verify sonar:sonar \
//   -Dsonar.projectKey=petclinic \
//   -Dsonar.projectName='petclinic' \
//   -Dsonar.host.url=http://localhost:9000 \
//   -Dsonar.token=sqp_48d1f8f6fd4795308fe817dd102227f8a3a0865d'''
//             }
//         }
        stage("Docker Push") {
            steps{
                script{
                  bat 'cd C:\\Program Files\\Jenkins\\workspace\\Maven' 
                  bat 'docker build -t pipeline . 
                }
                
            }
        }
    }
}
