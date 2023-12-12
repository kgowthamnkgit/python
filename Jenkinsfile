pipeline{
    options{
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '4'))
    }
    agent any
    stages{
        stage("Building Docker Image"){
            steps{
                echo "========executing Building Docker Image========"
                scripts{
                    checkout scm
                    def customImage = docker.build("my-image:${env.BUILD_ID}")
                    customImage.push()

                    customImage.push('latest')
                }
            }
        }
    }
}