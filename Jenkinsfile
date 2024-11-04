pipeline {
    agent any
     environment {
                SONAR_TOKEN = credentials('sonarqube')
        }
    stages {
        stage('Checkout GIT') {
            steps {
                git branch: 'ZarouiAhmed-5NIDS2-G6',
                url: 'https://github.com/ahmedzaroui1/5NIDS2-G6-TPFOYER.git'            
            }
        }
        stage('Maven Clean') {
            steps{
                sh "mvn clean"
            }
        }
        stage('Maven Compile') {
            steps{
                sh "mvn compile"
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_TOKEN')]) {
                    sh "mvn sonar:sonar -Dsonar.login=${SONAR_TOKEN} -Dsonar.projectKey=projet-jenkins -Dsonar.projectName='projet-jenkins'"
                }
            }
        }
        stage('Maven Package'){
            steps{
                sh "mvn package -DskipTests"
            }
        }

        stage('Login to Docker') {
            steps {
                script {
                    // Retrieve credentials from Jenkins securely
                    withCredentials([usernamePassword(
                        credentialsId: 'docker',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )]) {
                        // Login to Docker
                        sh """
                        docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                        """
                    }
                }
            }
        }
        stage('Building Docker Image'){
            steps{
                sh "docker build -t ahmedzaroui-5nids2-g6-tpfoyer ."
            }
        }
        stage('Tagging Docker Image'){
            steps{
                sh "docker tag ahmedzaroui-5nids2-g6-tpfoyer:latest zarouiahmeed/ahmedzaroui-5nids2-g6-tpfoyer:latest"
            }
        }
        stage('Pushing Docker Image'){
            steps{
                sh "docker push zarouiahmeed/ahmedzaroui-5nids2-g6-tpfoyer:latest"
            }
        }
    }
}