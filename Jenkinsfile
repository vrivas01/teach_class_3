pipeline {
    agent any

    environment {
         WEBSERVER = "Apache"
    }
    stages {
        stage('Create  directory for the WEB Application')
        {
            steps{
                sh 'rm -rf /home/jenkins/web/* /home/jenkins/web/.* || true'
            }
        }
        stage('Drop the container'){
            steps {
                echo 'droping the container...'
                sh 'docker rm -f app-web'
            }
        }

        /*
           Default local volume: C:/Users/vriva/jenkins_home/apache
        */

        // Apache Webserver
        stage('Create the Apache container') {
            when {
                 environment name: 'WEBSERVER', value: 'Apache'
            }
            steps {
                echo 'Creating the container...'
                sh 'docker run -dit --name app-web --network=jenkins -p 9000:80  -v C:/Users/vriva/jenkins_home/apache:/usr/local/apache2/htdocs/ httpd'
            }
        }

        //Nginx webserver
        stage('Create the Nginx container') {
            when {
                 environment name: 'WEBSERVER', value: 'Nginx'
            }
            steps {
                echo 'Creating the container...'
                sh 'docker run -dit --name app-web --network=jenkins -p 9100:80  -v C:/Users/vriva/jenkins_home/apache:/usr/share/nginx/html nginx'
            }
        }

        stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web application...'             
                sh 'cp -r web/* /home/jenkins/web'
            }
        }
    }
    post {
        always {
            echo 'These steps are always executed'   
        }
        success {
            // One or more steps need to be included within each condition's block.
            echo 'the deployment has worked'
            archiveArtifacts allowEmptyArchive: true, artifacts: 'web/*', followSymlinks: false
            cleanWs()
       }
       failure {
            // One or more steps need to be included within each condition's block.
            echo 'An error has ocurred'       
       }
    }
}
