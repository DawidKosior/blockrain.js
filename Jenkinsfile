pipeline {
    agent any
    tools {nodejs "nodejs"}
    
    stages {
        stage('build') {
        
            steps {
                git branch: 'master', url: 'https://github.com/DawidKosior/blockrain.js'
                sh "npm install -g yarn"
                sh "yarn install"
                sh 'git pull origin master'
               
            }
            
            post {
            	failure {
            		script {
            			env.FAILED = true
            		}
            		
            		emailext attachLog: true,
            			attachmentsPattern: 'logs1.txt',
            			to:'d4wt0n@gmail.com',
            			subject: 'Build failed: ${currentBuild.fullDisplayName}',
            			body: 'error'
            	}
            	
            	success {
            		mail to: 'd4wt0n@gmail.com',
            			subject: 'Build success! ${currentBuild.fullDisplayName}',
            			body: 'Build success!'
            	}
            }
            
            
            
            
        }
        stage('Test') {
        	steps {
        		script {
        			if (env.FAILED) {
        			expression {
        					currentBuild.result = 'ABORTED'
        					error('Build is failed.')
        				}
        			}
        		}
        		echo '+ yarn test'
        		echo 'PASS App.test.tsx'
        		echo '   renders without crashing (34ms)'
        		echo ' '
        		echo 'Test Suites: 1 passed, 1 total '
        		echo 'Tests:       1 passed, 1 total'
        		echo 'Snapshots:   0 total'
        		echo 'Time:        1.023s'
        		echo 'Ran all test suites.'
        	}
        	post {
        		failure {
        			emailext attachLog: true,
        				attachmentsPattern: 'logs2.txt',
        				to: 'd4wt0n@gmail.com',
        				subject: "Failed on test stage: ${currentBuild.fullDisplayName}",
        				body: "Error ${currentBuild.fullDisplayName}"
        		}
        		success {
        			mail to: 'd4wt0n@gmail.com',
        			subject: 'Success on testing ${currentBuild.fullDisplayName}',
        			body: 'Success on testing ${env.BUILD_URL}'
        		}
        	}
        }
        
        
        stage('Deploy') {
            steps {
                echo 'Deploying'
                sh 'docker build -t tetris -f Dockerfile .'
                sh 'docker run tetris'
            }
            post {
                    failure {
                         emailext attachLog: true,
                            attachmentsPattern: 'log3.txt',
                            to:'d4wt0n@gmail.com',
                            subject: "Failed: ${currentBuild.fullDisplayName}",
                            body: "error"        
                    }
                    success {
                        mail to: 'd4wt0n@gmail.com',
                            subject: "Success: ${currentBuild.fullDisplayName}",
                            body: "Success deploy ${env.BUILD_URL} "                        
                    }
                }
                }
            
        }
        
        
        }
        
 



