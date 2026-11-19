
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Maven Build') {
            steps {
                bat 'mvn clean compile'
            }
        }

        stage('Maven Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Generate WAR') {
            steps {
                bat 'mvn package -DskipTests'
                archiveArtifacts artifacts: 'target/*.war',
                                 fingerprint: true
            }
        }
        stage('Deploy to Tomcat') {
    steps {
        deploy adapters: [
            tomcat9(
                credentialsId: 'tomcat-credentials',
                url: 'http://localhost:8085'
            )
        ],
        contextPath: 'ticket-reservation',
        war: 'target/*.war'
    }
}
        
stage('Docker Build') {
    steps {
        bat 'docker build -t YOUR_DOCKERHUB_USERNAME/oltrs:latest .'
    }
}

stage('Docker Push') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'dockerhub-credentials',
            usernameVariable: 'kmitseali',
            passwordVariable: 'dckr_pat_yoTOjh3cLy6nxwYLfnPkmgKyvC8'
        )]) {
            bat '''
                echo %DOCKER_TOKEN% | docker login -u %DOCKER_USER% --password-stdin
                docker push %DOCKER_USER%/oltrs:latest
                docker logout
            '''
        }
    }
}

    }
}
