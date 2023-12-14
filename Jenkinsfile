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
        stage("Deploy to K8s cluster"){
            when{
                branch 'main'
            }
            steps{
                script{
                    sh "chmod +x tag.sh"
                    sh "./tag.sh $BRANCH_NAME-${BUILD_NUMBER}"
                    echo "========DEPLOYING IMAGE TO K8S========"
                    sh 'export KUBECONFIG=/home/k8s/.kube/config'
                    //sh 'kubectl apply -f /var/lib/jenkins/workspace/testing/ks_deployment.yaml'
                    sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.20.5/bin/linux/amd64/kubectl"'  
                    sh 'chmod u+x ./kubectl'  
                    sh './kubectl get pods'
                }
            }
            post{
                success{
                    echo "Successfully deployed to k8s"
                }
                failure{
                    echo "Failed deployed to k8s"
                }
            }
        }
    }
    post {
      always {
        emailext attachLog: 'true', body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}", recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], to: 'durgaakhil0211@gmil.com', subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
        }
    }
}