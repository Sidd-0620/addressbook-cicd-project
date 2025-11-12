pipeline {
    agent any

    tools {
        // Use Jenkins-installed tools (configure these in Global Tool Configuration)
        maven 'Maven'
        jdk 'JDK11'
    }

    environment {
        TOMCAT_USER = 'admin'
        TOMCAT_PASS = 'admin123'
        TOMCAT_HOST = 'http://3.87.131.131:9090/'   // 👈 Updated to 9090
        TOMCAT_WEBAPPS = '/var/lib/tomcat9/webapps'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Cloning source code from GitHub...'
                git 'https://github.com/Sidd-0620/addressbook-cicd-project.git'
            }
        }

        stage('Build with Maven') {
            steps {
                echo 'Building project using Maven...'
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo 'Deploying WAR file to Tomcat (port 9090)...'
                sh '''
                WAR_FILE=$(ls target/*.war)
                echo "WAR file generated: $WAR_FILE"

                # Stop Tomcat before deployment
                sudo systemctl stop tomcat9

                # Copy WAR to Tomcat webapps directory
                sudo cp $WAR_FILE $TOMCAT_WEBAPPS/

                # Start Tomcat after deployment
                sudo systemctl start tomcat9

                echo "Deployment completed successfully on port 9090!"
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful! Access the app at ${TOMCAT_HOST}/mywebapp"
        }
        failure {
            echo "❌ Deployment Failed. Check Jenkins console logs for details."
        }
    }
}
