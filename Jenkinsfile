pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-credentials')
        SONARQUBE_TOKEN = credentials('sonarqube-token') // token SonarQube
    }

    stages {
        stage('GIT') {
            steps {
                git branch: 'master',
                    url: 'git@github.com:Therealcydex/Ahmedwassim_hammouda_4SAE11.git'
            }
        }

        stage('Compile Stage') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('MNN SONARQUBE') {
            steps {
                sh '''
                    mvn sonar:sonar \
                      -Dsonar.projectKey=Ahmedwassim_4SAE11 \
                      -Dsonar.host.url=http://<IP_VM_Ubuntu>:9000 \
                      -Dsonar.login=$SONARQUBE_TOKEN
                '''
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
