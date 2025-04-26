pipeline {
    agent any
   
    stages {
        stage('Create directory for the WEB Application')
        {
            steps{
                sh 'rm -rf /home/jenkins/web/* /home/jenkins/web/.* || true'
            }
        }
        stage('Drop the containers'){
            parallel {
                stage('Drop Apache container'){
                    steps {
                        echo 'droping the Apache container...'
                        sh 'docker rm -f app-web-apache'
                    }
                }
                stage('Drop Nginx container'){
                    steps {
                        echo 'droping the Nginx container...'
                        sh 'docker rm -f app-web-nginx'
                    }
                }
            }
            steps {
                echo 'droping the container...'
                sh 'docker rm -f app-web-apache'
                sh 'docker rm -f app-web-nginx'
            }
        }
        stage('Create the containers in Parallel') {
            parallel {
                stage('Create the Apache container') {
                    steps {
                        echo 'Creating the Apache Container...'
                        sh 'docker run -dit --name app-web-apache --network=jenkins -p 9100:80  -v C:/Users/vriva/jenkins_home/apache:/usr/local/apache2/htdocs/ httpd'
                    }
                }
                stage('Create the Nginx container') {
                    steps {
                        echo 'Creating the Apache container...'
                        sh 'docker run -dit --name app-web-nginx --network=jenkins -p 9200:80  -v C:/Users/vriva/jenkins_home/apache:/usr/share/nginx/html nginx'         
                   }
                }
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
        success {
            // One or more steps need to be included within each condition's block.
            echo 'The deployment in Nginx and Apache has worked'
            archiveArtifacts allowEmptyArchive: true, artifacts: 'web/*', followSymlinks: false
            cleanWs()         
       }
       failure {
            // One or more steps need to be included within each condition's block.
            echo 'An error has ocurred in the deploy'       
       }
    }

}
