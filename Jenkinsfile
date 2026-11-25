
pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'Maven'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Verify JAR') {
            steps {
                sh '''
                    echo "Verification du JAR :"
                    find backend/target -maxdepth 1 -type f -name "*.jar"
                    test -n "$(find backend/target -maxdepth 1 -type f -name "*.jar")"
                '''
            }
        }
    }

    post {
        success {
            echo 'BUILD SUCCESS'
            archiveArtifacts artifacts: 'backend/target/*.jar',
                             fingerprint: true
        }
        failure {
            echo 'BUILD FAILED'
        }
    }
}
