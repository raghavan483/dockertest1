pipeline {
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf dockertest1'
            sh 'git clone https://github.com/raghavan483/dockertest1.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'cd  /var/lib/jenkins/workspace/pipelinetestprod/dockertest1'
            sh 'cp /var/lib/jenkins/workspace/pipelinetestprod/dockertest1/* /var/lib/jenkins/workspace/pipelinetestprod '
            sh 'docker build -t raghu3062/pipelinetestprod:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push raghu3062/pipelinetestprod:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://10.1.2.244:2375 stop featurewebapp2 || true'
            sh    'docker -H tcp://10.1.2.244:2375 run --rm -dit --name featurewebapp2 --hostname featurewebapp2 -p 8091:80 raghu3062/featurewebapp2:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://10.1.2.244:9002'
          }
        }

    }
}
