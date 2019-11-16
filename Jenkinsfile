pipeline {
    agent any
    stages {
        stage('Poll') {
            sh 'mvn clean verify -DskipITs=true';
            junit '**/target/surefire-reports/TEST-*.xml'
            archive 'target/*.jar'
        }
        stage('Poll') {
            sh 'mvn clean verify -DskipITs=true';
            junit '**/target/surefire-reports/TEST-*.xml'
            archive 'target/*.jar'
        }
        stage ('Integration Test'){
            sh 'mvn clean verify -Dsurefire.skip=true';
            junit '**/target/failsafe-reports/TEST-*.xml'
            archive 'target/*.jar'
        }
        stage ('Publish'){
            def server = Artifactory.server 'Default Artifactory Server'
            def uploadSpec = """{
                "files": [
                    {
                        "pattern": "target/hello-0.0.1.war",
                        "target": "example-project/${BUILD_NUMBER}/",
                        "props": "Integration-Tested=Yes;Performance-Tested=No"
                    }
                ]
            }"""
            server.upload(uploadSpec)
        }        
    }
    post { 
        success { 
            emailext (
              subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
              body: """<p>SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
              recipientProviders: [[$class: 'DevelopersRecipientProvider']]
            )
        }
        failure {
          emailext (
              subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
              body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
              recipientProviders: [[$class: 'DevelopersRecipientProvider']]
            )
        }
    }
}
