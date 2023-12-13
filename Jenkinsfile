pipeline{
    options{
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '4'))
    }
    environment{
        BRANCH_NAME = "${GIT_BRANCH.split("/")[1]}"
        dockerImage=''
        credentialsId= "DOCKER_HUB"
        docker_repository= 'kgowthamnk/docker-django-v0.0'
    }
    agent any
    stages{
        stage("Building Docker Image"){
            steps{
                script{
                    echo "========executing Building Docker Image========"
                    dockerImage = docker.build("$docker_repository:${BRANCH_NAME}-${BUILD_NUMBER}")
                }
            }
        }
        stage("Pushing Image to DockerHUB"){
            steps{
                script{
                    echo "========executing Docker Image PUSH========"
                    withDockerRegistry(credentialsId: 'DOCKER_HUB', url: '') {
                    dockerImage.push()
                    }
                }
            }
        }
    }
}