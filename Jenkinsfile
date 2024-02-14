pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/prabhatraghav/star-agile-health-care/', branch: "master"
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
        stage('Docker login and push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_cred', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo \$PASS | docker login -u \$USER --password-stdin"
                    sh 'docker push techomaniac83/medicureimgaddbook:latest'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def dockerCmd = 'docker run -itd --name medicure_container -p 80:8089 techomaniac83/medicureimgaddbook:latest'
                    sshagent(['sshkeypair']) {
                        def sshCmd = "ssh -o StrictHostKeyChecking=no techomaniac83@34.125.254.149 ${dockerCmd}"
                        echo "Executing SSH command: ${sshCmd}"
                        sh "${sshCmd}"
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline successfully executed!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
