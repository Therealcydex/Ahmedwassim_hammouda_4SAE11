pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }

    environment {
    	DOCKERHUB_CREDENTIALS = credentials('docker-hub-credentials')
    }

    stages {
        stage('GIT') {
            steps {
                git branch: 'master',
                    url: 'git@github.com:Therealcydex/Ahmedwassim_hammouda_4SAE11'
            }
        }

        stage('Compile Stage') {
            steps {
                sh 'mvn clean compile'
            }
        }

	 stage('Login to Docker Hub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

	stage('Build & Push Docker Image') {
            steps {
                script {
                    def dockerImage = 'wassimhamouda/student-management' 
                    
                    
                    sh "docker build -t $dockerImage ."
                    
                    
                    sh "docker push $dockerImage"
                }
            }
        }
    }
}
