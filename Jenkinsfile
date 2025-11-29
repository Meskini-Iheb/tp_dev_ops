pipeline {
    agent any

    stages {
        // Stage 1: Checkout - Manual Git clone to avoid Jenkins Git issues
        stage('Checkout') {
            steps {
                sh '''
                    echo "=== CHECKOUT STAGE ==="
                    echo "Downloading code from GitHub..."
                    rm -rf project-source
                    git clone https://github.com/Meskini-Iheb/tp_dev_ops.git project-source
                    cd project-source
                    pwd
                    ls -la
                    echo "Checkout completed successfully!"
                '''
            }
        }

        // Stage 2: Build - Compile and package the application
        stage('Build') {
            steps {
                sh '''
                    echo "=== BUILD STAGE ==="
                    cd project-source
                    mvn clean compile
                    echo "Build completed successfully!"
                '''
            }
        }

        // Stage 3: Tests - Run unit tests
        stage('Tests') {
            steps {
                sh '''
                    echo "=== TESTS STAGE ==="
                    cd project-source
                    mvn test
                    echo "Tests completed successfully!"
                '''
            }
            post {
                always {
                    junit 'project-source/target/surefire-reports/*.xml'
                }
            }
        }

        // Stage 4: SAST - Security scanning (simplified for now)
        stage('SAST') {
            steps {
                sh '''
                    echo "=== SAST STAGE ==="
                    cd project-source
                    echo "SAST Security Scan - This would run SonarQube in a real setup"
                    echo "For now, we'll run basic security checks..."
                    # Basic security checks
                    mvn compile
                    echo "SAST checks completed!"
                '''
            }
        }

        // Stage 5: Deploy - Deploy to Tomcat
        stage('Deploy') {
            steps {
                sh '''
                    echo "=== DEPLOY STAGE ==="
                    cd project-source
                    mvn clean package
                    echo "Deploying WAR file to Tomcat..."
                    sudo cp target/tp_dev_ops_dsi31.war /opt/tomcat/latest/webapps/
                    echo "Deployment completed successfully!"
                    echo "APPLICATION DEPLOYED!"
                    echo "Access your application at: http://localhost:8080/tp_dev_ops_dsi31/"
                '''
            }
        }
    }

    post {
        always {
            echo "=== PIPELINE EXECUTION COMPLETED ==="
            echo "Cleaning up workspace..."
            sh 'rm -rf project-source'
        }
        success {
            echo "PIPELINE SUCCEEDED!"
            echo "All stages completed successfully!"
            echo "Application is live at: http://localhost:8080/tp_dev_ops_dsi31/"
        }
        failure {
            echo "PIPELINE FAILED"
            echo "Check the stage where it failed above"
        }
    }
}
