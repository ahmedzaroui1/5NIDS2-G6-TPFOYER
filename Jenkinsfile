pipeline {
    agent any
    stages {
        stage('Checkout GIT') {
            steps {
                git branch: 'main',
                url: 'https://github.com/ahmedzaroui1/5NIDS2-G6-TPFOYER.git'            
            }
        }
        stage('Maven Clean') {
            steps{
                sh "mvn clean"
            }
        }
        stage('Maven Package'){
            steps{
                sh "mvn package -DskipTests"
            }
        }
    }
}