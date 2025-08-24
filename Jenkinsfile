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
		withdockerregistery([credentialsID:'docker-id', url:'']){
		sh 'docker image push mvnimage:$BUILD_NUMBER'
}
            }
        }
    }
}

