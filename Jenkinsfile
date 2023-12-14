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
                    echo "========PUSHING IMAGE TO DOCKERHUB========"
                    withDockerRegistry(credentialsId: 'DOCKER_HUB', url: '') {
                    dockerImage.push()
                    }
                }
            }
        }
        // stage("Deploy to K8s cluster"){
        //     when{
        //         branch 'main'
        //     }
        //     steps{
        //         sh "chmod +x tag.sh"
        //         sh "./tag.sh $BRANCH_NAME-${BUILD_NUMBER}"
        //     }
        //     script{
        //      echo "========DEPLOYING IMAGE TO K8S========"
        //         sh 'export KUBECONFIG=/home/ubuntu/.kube/config && kubectl apply -f /var/lib/jenkins/workspace/testing/ks_deployment.yaml'
        //     }
        //     post{
        //         success{
        //             echo "Successfully deployed to k8s"
        //         }
        //         failure{
        //             echo "Successfully deployed to k8s"
        //         }
        //     }
        // }
    }
}
