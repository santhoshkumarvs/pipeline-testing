pipeline {
    agent any
    stages {

        stage('Clone Repo') {
          steps {
             sh 'rm -rf pipeline-testing'
            sh 'git clone https://github.com/santhoshkumarvs/pipeline-testing.git
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'cd /var/lib/jenkins/workspace/pipeline2/pipeline-testing'
            sh 'cp  /var/lib/jenkins/workspace/pipeline2/pipeline-testing/* /var/lib/jenkins/workspace/pipeline2'
            sh 'docker build -t sreeharshav/pipelinetestmaster:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push sreeharshav/pipelinetestmaster:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://10.0.0.100:2375 stop masterwebapp1 || true'
            sh    'docker -H tcp://10.0.0.100:2375 run --rm -dit --name masterwebapp1 --hostname prodwebapp1 -p 8000:80 sreeharshav/pipelinetestmaster:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://10.0.0.100:8000'
          }
        }

    }
}
