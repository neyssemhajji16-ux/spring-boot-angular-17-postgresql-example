pipeline {
    agent any
    stages {
        stage('Git') {
            steps {
                git branch: 'master', url: 'https://github.com/neyssemhajji16-ux/spring-boot-angular-17-postgresql-example.git'
            }
        }
        stage('Maven Build') {
            steps {
                dir('spring-boot-server') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }
        stage('Building image') {
            steps {
                dir('spring-boot-server') {
                    sh 'docker build -t neyssem/spring-backend:1.0.0 .'
                }
            }
        }
        stage('Deploy Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
                    sh '''
                        echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin
                        docker push neyssem/spring-backend:1.0.0
                    '''
                }
            }
        }
        stage('Docker Compose') {
            steps {
                sh 'docker compose up -d'
            }
        }
    }
}
