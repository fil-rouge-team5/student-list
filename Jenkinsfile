pipeline  {
	environment {
		IMAGE_NAME = "student-list"
		IMAGE_TAG = "latest"
		IMAGE_REPO = "filrougeteam5"
		TEAM5_NETWORK= "jenkins-training_default"
	}
	agent none
	stages {
		stage('Build image') {
			agent any
			steps {
				script {
				     cd simple_api/
				     sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
				}
			}
		}
		stage('Run container based on builded image') {
			agent any
			steps {
				script {
					sh '''
					docker run --name $IMAGE_NAME -d -p 80:5000 --network $TEAM5_NETWORK -v /var/jenkins_home/workspace/deploiement-miniprojet/simple_api/student_age.json:/data/student_age.json $IMAGE_NAME:$IMAGE_TAG
					sleep 5
					'''
				}
			}
		}
		stage('Test container') {
			agent any
			steps {
				script {
					sh '''
					curl http://172.17.0.1
					'''
				}
			}
		}	
		
		stage('Cleaning Container') {
			agent any
			steps {
				script {
					sh '''
					docker rm -vf ${IMAGE_NAME}
						'''
				}
			}
		}
					
	stage('Push image on dockerhub') {
	           agent any 
	           environment {
	                DOCKERHUB_LOGIN = credentials('dockerhub_team5')
	            }
	           steps {
	               script {
	                   sh '''
			      docker login --username ${DOCKERHUB_LOGIN_USR} --password ${DOCKERHUB_LOGIN_PSW}
	                      docker push ${IMAGE_REPO}/${IMAGE_NAME}:${IMAGE_TAG}
	                   '''
	               }
	           }
	        }
	}
}
