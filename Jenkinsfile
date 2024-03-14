pipeline {
	agent any

	environment {
    	VERCEL_TOKEN = credentials('token')
	}

	stages {
    	stage('Build') {
        	steps {
            	git 'https://github.com/Nikhil-Sarwara/todo-app'
            	nodejs('node') {
                	sh 'npm install'
                	sh 'npm run build'
            	}
        	}
    	}

    	stage('Unit and Integration Tests') {
        	steps {
            	echo 'Running Unit and Integration Tests'
            	emailext attachLog: true, body: 'Unit and Integrations Tests are Done! Logs are attached with this email.', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'nikhil4818.be22@chitkara.edu.in'
        	}
    	}

    	stage('Code Analysis') {
        	steps {
         	script {
                	def scannerHome = tool 'SonarQube Scanner'
                	withSonarQubeEnv('sonarqube') {
                    	sh "${scannerHome}/bin/sonar-scanner \
                        	-Dsonar.projectKey=todo-app \
                        	-Dsonar.sources=. \
                        	-Dsonar.host.url=http://localhost:8094 \
                        	-Dsonar.login=sqp_4fffc8262c56144990a2d4aaf63530c9f89ed045"
                	}
            	}
        	}
    	}

    	stage('Security Scan') {
        	steps {
            	echo 'Security Scanning'
            	emailext attachLog: true, body: 'Security Scanning is Done! Logs are attached with this email.', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'nikhil4818.be22@chitkara.edu.in'
        	}
    	}

    	stage('Deploy to Staging') {
        	steps {
            	echo 'Deploying to staging area'
        	}
    	}

    	stage('Integration Tests on Staging') {
        	steps {
            	echo 'Running Integration Tests in Staging Area'
        	}
    	}

    	stage('Deploy to Production') {
        	steps {
            	echo 'Vercel Deployment'
            	sh 'vercel --token ${VERCEL_TOKEN} deploy -y'
        	}
    	}
	}
}



