pipeline {
    agent any
    stages {

        stage('Validate') {
	agent {
		label'node_for_vagrant_node'
}
            steps {
                echo 'Validating the code'
                sh 'mvn validate'
            }
        }

        stage('Compile') {
		 agent {
        	 label'node_for_vagrant_node'
}
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
				timeout(time:1, unit:'DAYS'){
				input message: 'Are you sure want to aprove deployment?'
}		
				echo 'Depolying in production level'
					sh '''
                       				 docker stop mvn2_app || true
                        			docker rm -f mvn2_app || true
                       				 docker run -dit -p 8088:8080 --name mvn2_app utsav0514/mvn_app:v1
                ''' 
}
}
    }
post {
  always {
       mail to: "utshavshrestha.1713@gmail.com"
	subject: "report on sucess or failure"
	body: "plese checkout the report ${BUILD_URL}"

}
success{
	mail to: "utsavshrestha2003@gmail.com"
	subject: "check the report of sucess"
	body: "The job name that sucess is  ${JOB_NAME}"
}
failure{
	mail to: "hamsurr@gmail.com"
	subject: "failure of the job"
	body: "plese check the job:  ${JOB_NAME}"

}
}
}

