pipeline {
    agent {label 'Devops-Agent'}
     environment {
                   key = credentials('openAI')
                }
    stages {
        stage('Code fetch') {
            steps {
                git url:'https://github.com/daipayanadhikari/chatgpt-web-application.git',branch:'master'
            }
        }
        stage('build and test') {
            steps {
                sh 'sudo docker build . -t daipayan/chatgpt-app:latest'
            }
        }
        stage('push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                sh 'docker push daipayan/chatgpt-app:latest'
            }
          }
        }
        stage('Deploy'){
            steps {
                withCredentials([string(credentialsId: 'openAI', variable: 'OPENAI_API_KEY')]) {
                                sh 'docker-compose down'
                                sh "docker-compose up -d"
                        }
                    
            }
        }
    }
}
