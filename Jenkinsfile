pipeline{
    options{
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '4'))
    }
    environment{
        BRANCH_NAME = "${GIT_BRANCH.split("/")[1]}"
    }
    agent any
    stages{
        stage("Building Docker Image"){
            steps{
                echo "========executing Building Docker Image========"
                sh 'docker build -t test:${BRANCH_NAME}-${BUILD_NUMBER} .'
            }
        }
    }
}