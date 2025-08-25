pipeline {
    agent any
    stages {

        stage('Validate') {
            steps {
                echo 'Validating the code'
                sh 'mvn validate'
            }
        }

        stage('Compile') {
            steps {
                echo 'Compiling the code'
                sh 'mvn compile'
            }
        }

        stage('Build and Package App') {
            steps {
                echo 'Building artifact'
                sh 'mvn package'
            }
            post {
                success {
                    echo 'Archiving the artifact'
                    archiveArtifacts artifacts: '**/*.war', followSymlinks: false, onlyIfSuccessful: true
                }
            }
        }

        stage('Creating Docker Image') {
            steps {
                echo 'Docker image created'
                sh 'whoami'
                sh 'docker build -t mvnimage:$BUILD_NUMBER .'
            }
        }

        stage('Scanning the Code with Trivy') {
            steps {
                echo 'Scanning the docker image with Trivy'
                sh 'trivy image -f json -o mvn-report.json mvnimage:$BUILD_NUMBER'
            }
        }
	 stage('push docker image to docker hub ') {
            steps {
		echo 'pusing the docker image'
		withDockerRegistry([credentialsId: 'dokcer-id', url: '']){
		sh 'docker tag mvnimage:$BUILD_NUMBER utsav0514/mvn_app:v1'
		sh 'docker push utsav0514/mvn_app:v1'
}
            }
        }
         stage('Deploying in local host'){
	steps{
		sh ''' 
			docker stop mvn_app || true 
			docker rm -f mvn_app || true 
			docker run -dit -p 8087:8080 --name mvn_app utsav0514/mvn_app:v1
		'''
}
}

	stage('Deploying in production level'){
	 steps{
		sh '''
                        docker stop mvn_app || true
                        docker rm -f mvn_app || true
                        docker run -dit -p 8088:8080 --name mvn_app utsav0514/mvn_app:v1
                ''' 
}
}
    }
}

