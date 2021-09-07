pipeline {
    agent any
    tools {nodejs "nodejs"}
    
    stages {
        stage('build') {
        
            steps {
                git branch: 'master', url: 'https://github.com/DawidKosior/blockrain.js'
                sh "npm install -g yarn"
                sh "npm install -g test"
                sh "yarn install"
                sh "yarn test install"
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
        		sh 'yarn run test > logs2.txt'
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
        }
        
    }



