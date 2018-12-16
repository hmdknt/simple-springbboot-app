#!/user/bin/env groovy

// Mattermost_Jenkins設定
def MATTERMOST_WEBHOOK = "http://"
def FAILURE_COLOR = "#FF0000"
def SUCCESS_COLOR = "#00BBFF"
def UNSTABLE_COLOR = "#FFEE00"

// Docker Image情報
def DOCKER_IMAGE_NAME='build-container'
def DOCKER_IMAGE_VERSION='1.0'


// Pipeline
pipeline {
    agent none
    stages {
        stage('Back-end') {
            agent {
                docker {
                    image "$NEXUS_HOST_NAME:$NEXUS_REPOSITORY_PORT/$DOCKER_IMAGE_NAME:$DOCKER_IMAGE_VERSION"
                    args "-e MYSQL_ROOT_PASSWORD=$ORG_GRADLE_PROJECT_inhouseRepositoryPassSub"
                    args "-p 3306:3306"
                }
            }
            steps {
                script {
                    if (env.BRANCH_NAME == 'master' || env.BRANCH_NAME == 'test') {
                        deleteDir()
                    }
                    checkout scm
                    env.SPRING_PROFILES_ACTIVE='test'
                    env.ORG_GRADLE_PROJECT_databaseJdbcSchema='sampledb'
                    env.TEST_DB_SCHEMA='sampledb'
                    env.PROJECT_RC_VERSION = new Date().format('yyyyMMddHHmm')
                }
            }
        }
    }
}



    // }
    // agent {
    //     docker {
    //         image "$NEXUS_HOST_NAME:$NEXUS_REPOSITORY_PORT/$DOCKER_IMAGE_NAME:$DOCKER_IMAGE_VERSION"
    //         args "-e MYSQL_ROOT_PASSWORD=$ORG_GRADLE_PROJECT_inhouseRepositoryPassSub"
    //         args "-p 3306:3306"
    //     }
    // }
    
    //options {
    //    gitLabConnection('GitLab')
    //}
    
    //triggers {
    //    gitlab(triggerOnPush: true, triggerOnMergeRequest: true, branchFilterType: 'All')
    //}
    
    // stages {
    //     stage('docker confirm') {
    //         steps {
    //             sh 'java -version'
    //         }
    //     }
    //     stage('clone source and setup env') {
    //         steps {
    //             script {
    //                 if (env.BRANCH_NAME == 'master' || env.BRANCH_NAME == 'release') {
    //                     deleteDir()
    //                 }
    //                 checkout scm
    //                 env.SPRING_PROFILES_ACTIVE='test'
    //                 env.ORG_GRADLE_PROJECT_databaseJdbcSchema='sampledb'
    //                 env.TEST_DB_SCHEMA='sampledb'
    //                 env.PROJECT_RC_VERSION = new Date().format('yyyyMMddHHmm')
    //             }
    //         }
    //     }
    //     stage('build, sonar, test report') {
    //         steps {
    //             script {
    //                 timeout(time: 1,unit: 'HOURS') {
    //                     if (env.BRANCH_NAME.startsWith('release')) {
    //                         sh './gradlew build -x test'
    //                     } else {
    //                         sh './gradlew build -x test'
    //                     }
    //                 }
    //             }
    //         }
    //     }
//        stage('Deploy to Nexus') {
//            steps {
//                script {
//                    if (env.BRANCH_NAME == 'master' || env.BRANCH_NAME.startsWith('release')) {
//                        echo "##################################################"
//                        echo "              Start Deploy to Nexus               "
//                        echo "##################################################"
//                        sh './gradlew publish'
//                    }
//                }
//            }
//        }
    // }
    
//    post {
//        always {
//            // sh 'docker recreate'
//            echo "ビルド終了しました"
//        }
//        failure {
//            updateGitlabCommitStatus name: "build", state: "failed"
//            mattermostSend endpoint: MATTERMOST_WEBHOOK, color: FAILURE_COLOR, message: "FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}: (<${env.BUILD_URL}|Open>)"
//        }
//        success {
//            updateGitlabCommitStatus name: "build", state: "success"
//            mattermostSend endpoint: MATTERMOST_WEBHOOK, color: SUCCESS_COLOR, message: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}: (<${env.BUILD_URL}|Open>)"
//        }
//        unstable {
//            mattermostSend endpoint: MATTERMOST_WEBHOOK, color: UNSTABLE_COLOR, message: "UNSTABLE: ${env.JOB_NAME} #${env.BUILD_NUMBER}: (<${env.BUILD_URL}|Open>)"
//        }
//    }
//}

