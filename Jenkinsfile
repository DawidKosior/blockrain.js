pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                git branch: 'master', url: 'https://github.com/DawidKosior/blockrain.js'
                sh 'npm install'
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
            			body: 'error: ${env.BUILD_URL}'
            	}
            	
            	success {
            		mail to: 'd4wt0n@gmail.com',
            			subject: 'Build success! ${currentBuild.fullDisplayName}',
            			body: 'Build success! ${env.BUILD_URL}'
            	}
            }
            
            
            
            
        }
    }
}


