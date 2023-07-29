pipeline {
    agent { label 'JDK-8' }
    options {
        timeout(time: 30, unit: 'MINUTES')
    }
    triggers {
        pollSCM('* * * * *')
    }
    tools {
        jdk 'JDK_8'
    }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/wakaleo/game-of-life.git',
                    branch: 'master'
            }
        }
        stage('build and package') {
            steps {
                sh script: 'mvn package'
            }
        }
        stage('reporting') {
            steps {
                archiveArtifacts artifacts: '**/gameoflife.war'
                junit testResults: '**/target/surefire-reports/TEST-*.xml'
            }
        }
    }
     post{
        success {
            mail subject:"${JOB_NAME}: has completed with success",
                 body: "your project is effective \n Build Url ${BUILD_URL}",
                 to: 'all@qt.com'
        }
        failure {
            mail subject:"${JOB_NAME}: has completed with failure",
                 body: "your project is deffective \n Build Url ${BUILD_URL}",
                 to: 'all@qt.com'
        }
    } 
    

} 