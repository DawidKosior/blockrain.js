pipeline {
    agent any
    tools {nodejs "nodejs"}
    stages {
        stage('build') {
            steps {
                git branch: 'master', url: 'https://github.com/DawidKosior/blockrain.js'
                sh '/usr/bin/npm install'
                sh 'git pull origin master'
                sh 'yarn build > logs1.txt'
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
    }
}


