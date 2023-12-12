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
                    docker.build
                }
            }
        }
    }
}