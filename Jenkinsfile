pipeline {
            environment {
                IMAGE_NAME = "student-list"
                IMAGE_TAG = "latest"
                IMAGE_REPO = "filrougeteam5"
             }
            agent none
            stages {
                stage('Build Image') {
                    agent any
                    steps {
                        script {
                            sh '''
                                   echo 'Building..'
                                   cd simple_api/
                                   docker build -t ${IMAGE_REPO}/${IMAGE_NAME}:${IMAGE_TAG} .
                            '''
                        }
                    }
                }
                stage('Run container based on builded image') {
                    agent any
                    steps {
                       script {
                         sh ''' docker run --name ${IMAGE_NAME} -d -p 80:5000 -v 
/home/centos/student-list/simple_api/student_age.json:/data/student_age.json ${IMAGE_REPO}/${IMAGE_NAME}:${IMAGE_TAG}
                            sleep 5
                         '''
                       }
                    }
                }
                stage('Test image') {
                    agent any
                    steps {
                        script {
                            sh '''
                                    curl http://172.17.0.1:80/ | grep -q "error"
                            '''
                                }
                        }
                }
                                stage('Clean Container') {
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
                                                           docker tag ${IMAGE_REPO}/${IMAGE_NAME}:${IMAGE_TAG} ${IMAGE_REPO}/${IMAGE_NAME}:${IMAGE_TAG}
                                                           docker login --username ${DOCKERHUB_LOGIN_USR} --password ${DOCKERHUB_LOGIN_PSW}
                                                           docker push ${IMAGE_REPO}/${IMAGE_NAME}:${IMAGE_TAG}
                                                           '''
                                                        }
                                                }
                                        }
                                         stage('Ansible deploy') {
                                            agent {
                                                docker {
                                                        image 'dirane/docker-ansible'
                                                } 
                                            }
                                            steps {
                                                script {
                                                    sh '''
                                                        cd ansible
                                                        ansible-playbook -i prod.yml install-docker.yml
                                                        ansible-playbook -i prod.yml studentlist.yml
                                                    '''
                                                }
                                            }
                                        }
                                }
                        }
        
