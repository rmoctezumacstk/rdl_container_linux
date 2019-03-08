pipeline {
    agent none
    stages {
		stage('Copy rdl from repository'){
			agent any
			steps{
				sh 'cp ./*.rdl ./docker_rdl'
			}
		}
		stage('Remove volumes, containers and images'){
			agent any
            steps{
                sh'''
					docker stop prototipo_admincontenido
					docker rm rdl_admincontenido
					docker rm prototipo_admincontenido
					docker rm screenshots_admincontenido
					
					docker rmi softtek:rdl-admincontenido
					docker rmi softtek:riot-admincontenido
					docker rmi softtek:screenshots-admincontenido

					docker volume rm v-rdl-admincontenido
                    docker volume rm v-screenshots-admincontenido
                    docker volume rm v-uml-admincontenido
					docker volume rm v-pdf-admincontenido
					
                    docker volume create --name v-rdl-admincontenido
                    docker volume create --name v-screenshots-admincontenido
                    docker volume create --name v-uml-admincontenido
					docker volume create --name v-pdf-admincontenido
				'''
            }
        }
		stage('RDL Generator'){
			agent any
            steps{
                sh '''
                    cd ./docker_rdl
                    docker build . -t "softtek:rdl-admincontenido"
                    docker run --name rdl_admincontenido -v v-rdl-admincontenido:/rdl/input/src-gen softtek:rdl-admincontenido
                '''
            }
        }
		stage('Prototype'){
			agent any
            steps{
                sh '''
                    cd ./docker_riot
					chmod +x copy.sh
                    docker build . -t  "softtek:riot-admincontenido"
                    docker run -d --name prototipo_admincontenido -p 172.16.68.31:1337:1337/tcp -v v-rdl-admincontenido:/rdl/input/src-gen softtek:riot-admincontenido
                '''
            }
        }
		stage('Screenshots'){
			agent any
            steps{
                sh '''
                    cd ./docker_screenshots
					chmod +x copy.sh
                    docker build . -t  "softtek:screenshots-admincontenido"
                    docker run --name screenshots_admincontenido -v v-rdl-admincontenido:/rdl/input/src-gen -v v-screenshots-admincontenido:/cypress/screenshots softtek:screenshots-admincontenido
				'''
            }
        }
    }
}
