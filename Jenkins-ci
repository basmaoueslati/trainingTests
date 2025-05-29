pipeline {
    agent any
    stages {
        //Continuous Integration
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests=true'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
                //Continuous Delivery
        stage('Docker Build&Push') {
            steps {
                withDockerRegistry(credentialsId: 'docker', url: "") {
                    sh 'docker build -t issaouib/compare-appf25 .'
                    sh 'docker push issaouib/compare-appf25'
                }
            }
        }
        //Continuous Deployment
        stage('Deploy') {
            steps {
                withDockerRegistry(credentialsId: 'docker', url: "") {
                    sh 'docker -H ssh://ubuntu@172.31.1.77 run -d --name myapp -p 8082:8080 issaouib/compare-appf25'
                }
            }  
        }
    }
}
