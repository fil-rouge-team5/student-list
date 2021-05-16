pipeline  {
    environment {
        IMAGE_NAME = "student-list"
        IMAGE_TAG = "latest"
        TEAM5_NETWORK= "jenkins-training_default"
    }
    agent none
    stages {
        stage('Build image') {
            agent any
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
                }
            }
        }
        stage('Run container based on builded image') {
            agent any
            steps {
                script {
                    sh '''
                    docker run --name $IMAGE_NAME -d -p 80:5000 -v ${PWD}/student_age.json:/data/student_age.json $IMAGE_NAME:$IMAGE_TAG
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
	
    }
}
