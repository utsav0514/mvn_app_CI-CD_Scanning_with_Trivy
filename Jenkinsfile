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
                sh '''
                    trivy image -f json -o mvn-report.json mvnimage:$BUILD_NUMBER
                    trivy image --severity HIGH,CRITICAL --exit-code 1 mvnimage:$BUILD_NUMBER
                '''
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo 'Pushing the docker image'
                withDockerRegistry([credentialsId: 'dokcer-id', url: '']) {
                    sh '''
                        docker tag mvnimage:$BUILD_NUMBER utsav0514/mvn_app:v1
                        docker push utsav0514/mvn_app:v1
                    '''
                }
            }
        }

        stage('Deploying in Localhost') {
            steps {
                sh '''
                    docker stop mvn_app || true
                    docker rm -f mvn_app || true
                    docker run -dit -p 8096:8080 --name mvn_app utsav0514/mvn_app:v1
                '''
            }
        }

        stage('Deploying in Production Level') {
            steps {
                timeout(time: 1, unit: 'DAYS') {
                    input message: 'Are you sure you want to approve deployment?'
                }
                echo 'Deploying in production level'
                sh '''
                    ansible-playbook deployment.yml
                '''
            }
        }
    }

    post {
        always {
            mail to: "utshavshrestha.1713@gmail.com",
                 subject: "Report on success or failure",
                 body: "Please checkout the report ${BUILD_URL}"
        }
        success {
            mail to: "utsavshrestha2003@gmail.com",
                 subject: "Check the report of success",
                 body: "The job that succeeded is ${JOB_NAME} ${BUILD_URL}"
        }
        failure {
            mail to: "ujwalshresthakanxhu.1713@gmail.com",
                 subject: "Failure of the job",
                 body: "Please check the job: ${JOB_NAME} ${BUILD_URL}"
        }
    }
}

