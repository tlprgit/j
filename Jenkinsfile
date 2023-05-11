pipeline {
    agent 'any'
    tools {
        git 'git'
    }
    stages {
        stage ('git pull') {
            steps {
                echo 'PULLING CODE FORM GITHUB'
                sh 'git pull https://github.com/tlprgit/jdal.git'
            }
        }
        stage ('LOG INTO ECR AND BUILD DOCKER IMAGES AND PUSH INTO ECR REPOS') {
            steps {
                echo 'BUILD AND PUSHING CONTAINER 1 TO ECR'
                sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 715739452629.dkr.ecr.ap-south-1.amazonaws.com'
                sh 'docker build -t cont1 ./cont1/Dockerfile'
                sh 'docker tag cont1:latest 715739452629.dkr.ecr.ap-south-1.amazonaws.com/cont1:latest'
                sh 'docker push 715739452629.dkr.ecr.ap-south-1.amazonaws.com/cont1:latest'
                echo 'BUILD AND PUSHING CONTAINER 2 TO ECR'
                sh 'docker build -t cont2 ./cont1/Dockerfile'
                sh 'docker tag cont2:latest 715739452629.dkr.ecr.ap-south-1.amazonaws.com/cont2:latest'
                sh 'docker push 715739452629.dkr.ecr.ap-south-1.amazonaws.com/cont2:latest'
            }
        }
        stage ('INSTALL TOMCAT ON REMOTE INSTANCES AND START CONTAINERS ON THEM') {
            steps {
                sh 'ansible-playbook tomcatserver.yml --syntax-check'
                sh 'ansible-playbook tomcatserver.yml'
            }
        }
        post {
            echo 'PROCESS COMPLETED CHECK IN BROWSERS'
        }
    }
}