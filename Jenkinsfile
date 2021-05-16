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
				     sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG simple_api'
				}
			}
		}
		stage('Run container based on builded image') {
			agent any
			steps {
				script {
					sh '''
		docker run --name $IMAGE_NAME -d -p 35:5000 --network $TEAM5_NETWORK -v "$WORKSPACE"/student_age.json:/data/student_age.json $IMAGE_NAME:$IMAGE_TAG
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
					curl -I http://172.17.0.1:35
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
	}
}
