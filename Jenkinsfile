pipeline {
    agent any
    tools {
        maven "maven_3_9_6"
    }
    stages {
        stage("mvn test") {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Matt-Hays/csi5324-jenkins-assignment']])
                    sh "mvn -version"

                    dir("/Users/matthewhays/Desktop/Active Coursework/CSI 5324/Week 7/assignment7.2/jenkins-tutorial") {
                        sh "mvn clean install"
                    }
            }
        }
        stage("Build Docker File") {
            steps {
                dir("/Users/matthewhays/Desktop/Active Coursework/CSI 5324/Week 7/assignment7.2/jenkins-tutorial") {
                    sh "docker build -t matthays/presentation_hub ."
                }
            }
        }
        stage("Push image to dockerhub") {
            steps {
                script {
                    withCredentials([string(credentialsId: '6876cbac-69b0-4564-812f-b8d39588b524', variable: 'password')]) {
                        sh "docker login -u matthays -p %password%"
                        sh "docker push matthays/presentation_hub"
                    }
                }
            }
        }
    }
    post {
        always {
            clearWs()
        }
    }
}