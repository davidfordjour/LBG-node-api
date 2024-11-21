pipeline {
	agent any
	stages{
		stage('Build'){
			steps{
			sh '''
			docker build -t davidfordj98/task2jenkins:latest -t davidfordj98/task2jenkins:v${BUILD_NUMBER} .
			'''
			}
		}
		stage('Push'){
			steps{
            sh '''
            docker push davidfordj98/task2jenkins:latest
            docker push davidfordj98/task2jenkins:v${BUILD_NUMBER}
			'''
			}
		}
		stage('Deploy'){
			steps{
			sh '''
            kubectl apply -f ./kubernetes

            kubectl set image deployment/api-deployment api-container=davidfordj98/task2jenkins:v${BUILD_NUMBER}
			'''
			}
		}
    }
}