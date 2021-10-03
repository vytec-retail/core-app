pipeline {
    agent any
    tools {
        maven "M3"
    }

    stages {
        stage('Build') {
            steps {
                git branch: 'main', url: 'https://github.com/vytec-retail/core-app.git'
                sh "mvn -f pom.xml -Dmaven.test.failure.ignore=true clean deploy -s settings.xml"
            }

            post {
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        stage('deploy') {
              agent { label 'k8s-label' }
            steps {
                input 'aproove to deploy app into k8s'
                echo "deploy into dev k8s cluster"
                git branch: 'master', url: 'https://github.com/qf-devops/k8s-microservices.git'
                sh '/usr/bin/kubectl apply -f k8s-microservices/kubernetes/'
            }
        }
    }
}
