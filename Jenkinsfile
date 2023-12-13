pipeline{
    options{
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '4'))
    }
    environment{
        BRANCH_NAME = "${GIT_BRANCH.split("/")[1]}"
        dockerImage=''
        credentialsId= "DOCKER_HUB"
       // url= 'https://hub.docker.com/repositories/kgowthamnk'
    }
    agent any
    stages{
        stage("Building Docker Image"){
            steps{
                script{
                    echo "========executing Building Docker Image========"
                    //sh 'docker build -t test:${BRANCH_NAME}-${BUILD_NUMBER} .'
                    dockerImage = docker.build("test:${BRANCH_NAME}-${BUILD_NUMBER}")
                }
            }
        }
        stage("Pushing Image to DockerHUB"){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'DOCKER_HUB', url: '') {
                    dockerImage.push()
                    }
                }
            }
        }
    }
}