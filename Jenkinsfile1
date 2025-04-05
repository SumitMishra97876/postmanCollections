pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo "Building the war"
            }
        }

        stage('Deploy to QA') {
            steps {
                echo "Deploying to QA"
            }
        }

        stage('Pull Docker Images') {
          
                    steps {
                        bat 'docker pull sumitmishra863756/datadriventest:1.0'
                    }
                }
                
                
        

        stage('Prepare Newman Results Directory') {
            steps {
                bat 'mkdir "newman"'
            }
        }

        stage('Run API Test Cases') {
           
                    steps {
                        bat 'docker run --rm -v %cd%\\newman:/app/results sumitmishra863756/datadriventestbookings:1.0'
                    }
                }
               

        stage('Publish HTML Extra Reports') {
            
                    steps {
                        publishHTML([
                            allowMissing: false,
                            alwaysLinkToLastBuild: false,
                            keepAll: true,
                            reportDir: 'newman',
                            reportFiles: 'booking.html',
                            reportName: 'GoRest API Report',
                            reportTitles: ''
                        ])
                    }
                }
                
                

        stage('Deploy to PROD') {
            steps {
                echo "Deploying to PROD"
            }
        }
    }
}
