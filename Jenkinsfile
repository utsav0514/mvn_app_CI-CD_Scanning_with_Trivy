 

pipeline {
	agent any 
	stages{

		stage('validte'){
                        steps{
                        echo 'validating the code'
                        sh 'mvn validate'
} 
}
		stage('compile'){
			steps{
			echo 'compiling the code'
			sh 'mvn compile'
}	
}
		stage('unit test'){
                        steps{
                        echo 'test sucessful'
}
}
		stage('Build and package app'){
                        steps{
                        echo 'Building artifact'
}
}
		stage('uplaod artifact'){
                        steps{
                        echo 'uploading artifact'
}
}
		stage('creating docker image'){
                        steps{
                        echo 'docker image created'
}
}
		stage('scanning docker images'){
                        steps{
                        echo 'Docker images scann sucessful'
}
}
}
}
	
