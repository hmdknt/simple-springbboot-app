#!/user/bin/env groovy


// Pipeline
pipeline {
    agent any
    stages {
        stage('clone source and setup env') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'master' || env.BRANCH_NAME == 'release') {
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
        stage('build, sonar, test report') {
            steps {
                script {
                    timeout(time: 1,unit: 'HOURS') {
                        if (env.BRANCH_NAME.startsWith('release')) {
                            sh './gradlew build -x test'
                        } else {
                            sh './gradlew build -x test'
                        }
                    }
                }
            }
        }
    }
}