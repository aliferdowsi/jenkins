pipeline {
    agent {
        label 'jdk21-maven-git-docker' // Label for your Jenkins agent that has Java and Maven
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
                sh 'docker build -t law12345/app-jenkins:testserver .'
            }
        }
        stage('Docker Login & Push') {
            steps {
                script {
                    // Use credentials stored in Jenkins to log in
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', 
                                                     usernameVariable: 'DOCKER_USER', 
                                                     passwordVariable: 'DOCKER_PASS')]) {
                        // Log in to Docker Hub
                        sh """
                            echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        """
                    }

                    // Push the image to Docker Hub
                    sh "docker push law12345/app-jenkins:testserver"
                }
            }
        }
        stage('SSH to Host System') {
            steps {
                script {
                    // Use SSH credentials stored in Jenkins (ID is 'host-ssh-key')
                    sshagent(['host-ssh-key']) {
                        sh '''
                            ssh -o StrictHostKeyChecking=no your_host_user@localhost "echo 'Connected to host system'; uptime"
                        '''
                    }
                }
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
