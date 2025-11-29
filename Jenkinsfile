pipeline {
    agent any

    tools {
        maven 'Maven-3.9.11'
        jdk 'jdk21'
        git 'system-git'
    }

    stages {
        // Stage 1: Checkout Code from GitHub
        stage('Checkout') {
            steps {
                git branch: 'master', 
                url: 'https://github.com/Meskini-Iheb/tp_dev_ops.git'
            }
        }

        // Stage 2: Build with Maven
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        // Stage 3: Run Unit Tests
        stage('Tests') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        // Stage 4: Package the application
        stage('Package') {
            steps {
                sh 'mvn clean package'
            }
        }

        // Stage 5: Deploy to Tomcat
        stage('Deploy') {
            steps {
                sh '''
                    echo "Stopping Tomcat..."
                    sudo systemctl stop tomcat
                    
                    echo "Deploying WAR file..."
                    sudo cp target/tp_dev_ops_dsi31.war /opt/tomcat/latest/webapps/
                    
                    echo "Starting Tomcat..."
                    sudo systemctl start tomcat
                    
                    echo "Deployment completed!"
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed - checking results...'
        }
        success {
            echo 'Pipeline succeeded! Application deployed successfully.'
        }
        failure {
            echo 'Pipeline failed! Check the logs for details.'
        }
    }
}
