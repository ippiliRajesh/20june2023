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
        maven 'MAVEN_DEFAUL'
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
                 rtMavenDeployer (
                    id: "GOF_DEPLOYER",
                    serverId: "MY_FREE_JFROG",
                    releaseRepo: 'devops-libs-snapshot-local',
                    snapshotRepo: 'devops-libs-snapshot-local'
                )
                rtMavenRun (
                    tool: 'MAVEN_DEFAUL', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "GOF_DEPLOYER"
                    //,
                    //buildName: "${JOB_NAME}",
                    //buildNumber: "${BUILD_ID}"
                )
                rtPublishBuildInfo (
                    serverId: "MY_FREE_JFROG"
                )
            }
        }
       
        stage('reporting') {
            steps {
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


