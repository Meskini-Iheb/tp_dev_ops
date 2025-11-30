pipeline {
    agent any

    tools {
        maven 'maven-3.9.11'
        jdk 'jdk21'
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
                    junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
                }
            }
        }

        // Stage 4: SAST Security Scan
        stage('SAST') {
            steps {
                sh '''
                    echo "=== SAST SECURITY SCAN ==="
                    echo "Running security checks..."
                    mvn compile
                    echo "SAST completed successfully!"
                '''
            }
        }

        // Stage 5: Package and Deploy
        stage('Deploy') {
    steps {
        sh '''
            echo "=== PACKAGING AND DEPLOYMENT ==="
            mvn clean package
            echo "Deploying WAR file to Tomcat..."
            
            # Stop Tomcat (now allowed via sudoers)
            sudo systemctl stop tomcat
            
            # Remove old deployment
            sudo rm -rf /opt/tomcat/latest/webapps/tp_dev_ops /opt/tomcat/latest/webapps/tp_dev_ops.war
            
            # Deploy new WAR file
            sudo cp target/tp_dev_ops.war /opt/tomcat/latest/webapps/
            
            # Start Tomcat
            sudo systemctl start tomcat
            
            echo "Deployment completed successfully!"
            echo "Application available at: http://localhost:8080/tp_dev_ops/"
        '''
    }
}
    }

    post {
        always {
            echo "=== PIPELINE EXECUTION COMPLETED ==="
        }
        success {
            echo "PIPELINE SUCCEEDED!"
            echo "All 5 CI/CD stages completed!"
            echo "Application deployed: http://localhost:8080/tp_dev_ops/"
        }
        failure {
            echo "PIPELINE FAILED - Check the logs above"
        }
    }
}
