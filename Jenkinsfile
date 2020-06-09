pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            when{
                  branch 'master'
            }
            steps {
                echo 'Building docker image'
                script {
                    app = docker.build('oluwandy122/train-schedule')
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
        
        stage('Push image to registry') {
            when {
                branch 'master'
            }
            step {
                script {
                   docker.withRegistry('registry.hub.docker.com', 'docker_hub_login')
                   docker.push("${env.BUILD_NUMBER}")
                   docker.push("latest") 
                }
            }
        }
    }
}
