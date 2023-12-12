pipeline{
    options{
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '4'))
    }
    stages{
        stage("Building Docker Image"){
            steps{
                script{
                    docker.build 
                }
            }
        }
    }
}