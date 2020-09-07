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
		    sh 'cd /var/lib/jenkins/workspace/pipeline2/dockertest1'
			sh 'cp /var/lib/jenkins/workspace/pipeline2/dockertest1/* /var/lib/jenkins/workspace/pipeline2'
            sh 'docker build -t raman143/pipelinetestprod:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push raman143/pipelinetestprod:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://10.1.1.200:2375 stop prodwebapp1 || true'
            sh    'docker -H tcp://10.1.1.200:2375 run --rm -dit --name prodwebapp1 --hostname prodwebapp1 -p 8000:80 raman143/pipelinetestprod:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://10.1.1.200:8000'
          }
        }

    }
}
