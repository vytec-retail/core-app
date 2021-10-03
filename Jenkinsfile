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
        stage('Build2') {
            steps {
                input 'aproove the build to deploy'
                echo "deploy into dev k8s cluster"
            }
        }
    }
}
