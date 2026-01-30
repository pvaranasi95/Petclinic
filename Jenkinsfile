pipeline {
    agent any
        environment {
    ARTIFACTORY_CRED = credentials('Jfrog_Artifactory')
}


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

        // stage('Maven test') {
        //     steps {
        //         bat "mvn test"
        //     }
        // }

        // stage('Maven Package') {
        //     steps {
        //         bat "mvn clean install"
        //     }
        // }

//         stage('Sonar scan') {
//     steps {
//         withCredentials([string(credentialsId: 'Sonar', variable: 'SONAR_TOKEN')]) {
//             bat """
//             mvn -U verify org.sonarsource.scanner.maven:sonar-maven-plugin:3.11.0.3922:sonar ^
//              -Dsonar.projectKey=petclinic ^
//              -Dsonar.projectName=petclinic ^
//              -Dsonar.host.url=http://localhost:9000 ^
//              -Dsonar.token=%SONAR_TOKEN%
//             """
//         }
//     }
// }
        stage('Packaging the files') {
            steps {
                powershell '''
                    $source = Join-Path $env:WORKSPACE '*'
                    $destination = Join-Path $env:WORKSPACE '$($env:BUILD_NUMBER).zip'
                    Compress-Archive -Path $source -DestinationPath $destination -Force
                    '''
            }
        }
        
        stage('Publish to Artifactory') {
    steps {
        bat """
            jf rt u "*.zip" "%JOB_NAME%/%BUILD_NUMBER%/" --user=%ARTIFACTORY_CRED_USR% --password=%ARTIFACTORY_CRED_PSW% --url=http://localhost:8082/artifactory/
        """
    }
}

        stage('Verify Upload') {
    steps {
        bat """
        jf rt u "*.zip" "%JOB_NAME%/%BUILD_NUMBER%/" --user=%ARTIFACTORY_CRED_USR% --password=%ARTIFACTORY_CRED_PSW% --url=http://localhost:8082/artifactory/
        """
    }
}


    }
}
