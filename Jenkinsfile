pipeline {
	agent any
	stages{
		stage('Init') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/main') {
                        sh '''
                        kubectl create ns prod || echo "------- Prod Namespace Already Exists -------"
                        '''
                    } else if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        kubectl create ns dev || echo "------- Prod Namespace Already Exists -------"
                        '''
                    } else {
                        sh'echo "Unrecognised branch"'
                    }
                }
            }
        }
		stage('Build'){
			steps {
			    script {
                    if (env.GIT_BRANCH == 'origin/main') {
                        sh '''
                        docker build -t davidfordj98/task2jenkins:latest -t davidfordj98/task2jenkins:v${BUILD_NUMBER} .
                        '''
                    } else if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        docker build -t davidfordj98/task2jenkins:latest -t davidfordj98/task2jenkins:v${BUILD_NUMBER} .
                        '''
                    } else {
                        sh'echo "Unrecognised branch"'
                    }
                }
			}
		}
		stage('Push'){
			steps{
                script {
                    if (env.GIT_BRANCH == 'origin/main') {
                        sh '''
                        docker push davidfordj98/task2jenkins:latest
                        docker push davidfordj98/task2jenkins:v${BUILD_NUMBER}
			            '''
                    } else if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        docker push davidfordj98/task2jenkins:latest
                        docker push davidfordj98/task2jenkins:v${BUILD_NUMBER}
			            '''
                    } else {
                        sh'echo "Unrecognised branch"'
                    }
                }
			}
		}
		stage('Deploy'){
			steps{
			    script {
                    if (env.GIT_BRANCH == 'origin/main') {
                        sh '''
                        kubectl apply -f ./kubernetes

                        kubectl set image deployment/api-deployment api-container=davidfordj98/task2jenkins:v${BUILD_NUMBER}
			            '''
                    } else if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        kubectl apply -f ./kubernetes

                        kubectl set image deployment/api-deployment api-container=davidfordj98/task2jenkins:v${BUILD_NUMBER}
			            '''
                    } else {
                        sh'echo "Unrecogognised branch"'
                    }
                }
			}
		}
    }
}