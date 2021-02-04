pipeline {
			agent any
			environment {        
				DOCKER_HUB_REPO = "sarankaja/casestudy"
				REGISTRY_CREDENTIAL = "dockerhub"
				CONTAINER_NAME = "flaskapp"	
			}
			stages {
				stage('Clean workspace'){
					steps{
						script{
							sh 'rm -rf $PWD/2020_03_DO_Boston_casestudy_part_1'						
						}
					}
				}
				stage('Cloning our Git'){
					steps {
						script{
							sh 'git clone https://github.com/Kajasaran/2020_03_DO_Boston_casestudy_part_1.git' 
						}		
					}
				}
				stage('Building new image ') {
					steps {
						script {
							sh 'docker image build -t $DOCKER_HUB_REPO:latest .'
							sh 'docker image tag $DOCKER_HUB_REPO:latest $DOCKER_HUB_REPO:$BUILD_NUMBER'
							echo "image buit successfuly"
						}
					}	
				}
			       stage('Push Docker Image') {
               				 steps {
                   				withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]){

                    						sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                    						sh "docker tag sarankaja/casestudy"
                    						sh 'docker push sarankaja/casestudy'
                 }
             }
         }
              
		
				stage('Deploy to kubernetes'){
					steps{
						ansiblePlaybook credentialsId: 'kubernetes', disableHostKeyChecking: true, installation: 'ansible', playbook: 'playbook.yaml'
					}
				}
			}
		}
