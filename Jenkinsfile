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
            parallel {
                stage('Pull Booking Image') {
                    steps {
                        bat 'docker pull sumitmishra863756/datadriventestbookings:1.0'
                    }
                }
                
                stage('Pull GoRest Image') {
                    steps {
                        bat 'docker pull sumitmishra863756/apichainingtest:2.0'
                    }
                }
            }
        }

        stage('Prepare Newman Results Directory') {
            steps {
                bat 'mkdir "newman"'
            }
        }

        stage('Run API Test Cases in Parallel') {
            parallel {
                stage('Run Bookings Tests') {
                    steps {
                        bat 'docker run --rm -v %cd%\\newman:/app/results sumitmishra863756/datadriventestbookings:1.0'
                    }
                }
                
                stage('Run GoRest Tests') {
                    steps {
                        bat 'docker run --rm -v %cd%\\newman:/app/results sumitmishra863756/apichainingtest:2.0'
                    }
                }
            }
        }

        stage('Publish HTML Extra Reports') {
            parallel {
                stage('Publish Bookings Report') {
                    steps {
                        publishHTML([
                            allowMissing: false,
                            alwaysLinkToLastBuild: false,
                            keepAll: true,
                            reportDir: 'newman',
                            reportFiles: 'booking.html',
                            reportName: 'Bookings API Report',
                            reportTitles: ''
                        ])
                    }
                }
                
                stage('Publish GoRest Report') {
                    steps {
                        publishHTML([
                            allowMissing: false,
                            alwaysLinkToLastBuild: false,
                            keepAll: true,
                            reportDir: 'newman',
                            reportFiles: 'gorest.html',
                            reportName: 'GoRest API Report',
                            reportTitles: ''
                        ])
                    }
                }
            }
        }

        stage('Deploy to PROD') {
            steps {
                echo "Deploying to PROD"
            }
        }
    }
}
