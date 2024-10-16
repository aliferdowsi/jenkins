pipeline {
    agent {
        label 'jdk21-maven-git' // Label for your Jenkins agent that has Java and Maven
    }
    
    environment {
        MAVEN_HOME = "/opt/apache-maven-3.9.4" // Path to Maven (ensure it matches where Maven is installed in the Docker image)
        PATH = "${MAVEN_HOME}/bin:${PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Check out the code from SCM (Jenkins will use SCM info from job configuration)
                checkout scm
            }
        }
        stage('Build Spring-Boot') {
            steps {
                // Run Maven to build the project
                sh 'mvn clean install'
            }
        }
        stage('Build Docker Image') {
            steps {
                // Run unit tests (if there are any in your project)
                sh 'docker build -t law12345/test-jenkins:latest .'
            }
        }
        stage('Image ls') {
            steps {
                // Run unit tests (if there are any in your project)
                sh 'docker image ls'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        success {
            echo 'Build was successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
