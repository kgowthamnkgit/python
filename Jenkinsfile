pipeline{
    options{
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '4'))
    }
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