pipeline {
    agent any
        environment {
    ARTIFACTORY_CRED = credentials('Jfrog_Artifactory')
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
        
//         stage('Publish to Artifactory') {
//     steps {
//         bat """
//             curl.exe -u %ARTIFACTORY_CRED_USR%:%ARTIFACTORY_CRED_PSW% -T "target\\petclinic.war" "http://localhost:8081/artifactory/%JOB_NAME%/%BUILD_NUMBER%/petclinic.war"
//         """
//     }
// }

//         stage('Verify Upload') {
//     steps {
//         bat """
//         curl.exe -u %ARTIFACTORY_CRED_USR%:%ARTIFACTORY_CRED_PSW% "http://localhost:8081/artifactory/api/storage/Test1/%JOB_NAME%/%BUILD_NUMBER%/"
//         """
//     }
// }
    }
    post{
        always{
            script {
                    def jenkinsBuildData = [
                job_name: env.JOB_NAME,
                build_number: env.BUILD_NUMBER.toInteger(),
                status: currentBuild.currentResult,
                timestamp: new Date().format("yyyy-MM-dd'T'HH:mm:ss.SSS'Z'", TimeZone.getTimeZone('UTC')),
                duration: currentBuild.duration,
                url: env.BUILD_URL
            ]

            def jsonBody = groovy.json.JsonOutput.toJson(jenkinsBuildData)
            def jsonBodyEscaped = jsonBody.replace('"', '\\"')

            echo "Sending build data to Elasticsearch: ${jsonBody}"

            bat """
            curl.exe -X POST "http://localhost:9200/jenkins/_doc" ^
                 -H "Content-Type: application/json" ^
                 -d "${jsonBodyEscaped}"
            """
                }

        }
    }
}
