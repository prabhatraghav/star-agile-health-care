pipeline {
    agent any
    stages {
        stage('checkout') {
            steps {
                git url:'https://github.com/prabhatraghav/star-agile-health-care/', branch: "master"
            }
        }
        stage('Build') {
            steps {
               sh "mvn clean package"
            }
        }
       
        stage('Build Image') {
            steps {
                sh 'docker build -t medicureimg .'
                sh 'docker tag medicureimg:latest techomaniac83/medicureimgaddbook:latest'
            }
        }
        stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_cred', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push techomaniac83/medicureimgaddbook:latest'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def dockerCmd = 'docker run -itd --name medicure_container -p 80:8089 techomaniac83/medicureimgaddbook:latest'
                    sshagent(['sshkeypair']) {
                        sh "ssh -o StrictHostKeyChecking=no techomaniac83@34.125.140.254 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
