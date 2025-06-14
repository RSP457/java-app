pipeline {
    agent any
    
    tools {
        maven 'maven-3.9'
    }

    stages {
        stage('Build jar') {
            steps {
                echo "Building the jar file using Maven..."
                sh 'mvn package'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    echo "Building the Docker image..."

                    // Menggunakan withCredentials untuk login ke Docker registry
                    withCredentials([usernamePassword(credentialsId: '7b4376a3-0bc6-49af-adc8-fa03b64648f0', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        // Build Docker image hanya sekali
                        sh 'docker build -t azeyna/demo-app:jma-2.0 .'
                        
                        // Login ke Docker registry dengan kredensial
                        sh "echo \$PASS | docker login -u \$USER --password-stdin"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying the application..."

                    // Deploy logic goes here, misalnya docker push atau ke server lain
                    // sh 'docker push azeyna/demo-app:jma-2.0'
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished"
        }
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check the logs for errors."
        }
    }
}
