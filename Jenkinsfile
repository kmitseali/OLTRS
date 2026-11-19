
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
    }
}
