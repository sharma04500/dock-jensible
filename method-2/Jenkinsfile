pipeline {
    agent {
        label 'vikram'
    }
    stages {
        stage('checkout the presence of source code') {
            steps {
        script {
            if (!fileExists('/home/ubuntu/food1')) {
                sh "cd /home/ubuntu && git clone https://github.com/sharma04500/food1.git"
            }
        }
    }
        }
        stage('pull the latest version of code') {
            steps {
                sh 'cd /home/ubuntu/food1 && git pull origin main || exit 1'
            }
        }
        stage('check for the running containers') {
            steps {
                sh 'docker container ps'
            }
        }
        stage('Stop & Delete the existing containers') {
            steps {
                sh 'for i in `docker container ps -aq`;do docker container stop $i && docker container rm -f $i;done'
            }
        }
        stage('Delete the existing images') {
            steps {
                sh 'for i in `docker image ls -aq`;do docker image rm -f $i;done'
            }
        }
        stage('Build new image from the latest code') {
            steps {
                sh 'cd /home/ubuntu && docker image build . -t myimage:latest'
            }
        }
        stage('Launch a new container') {
            steps {
                sh 'docker container run -d --name food -p9999:80 myimage:latest'
            }
        }
    }
}