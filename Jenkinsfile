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
					docker stop pdf_admincontenido
					docker rm rdl_admincontenido
					docker rm prototipo_admincontenido
					docker rm screenshots_admincontenido
					docker rm plantuml_admincontenido
					docker rm latex_admincontenido
					docker rm pdf_admincontenido
					
					docker rmi softtek:rdl-admincontenido
					docker rmi softtek:riot-admincontenido
					docker rmi softtek:screenshots-admincontenido
					docker rmi softtek:plantuml-admincontenido
					docker rmi softtek:latex-admincontenido
					docker rmi softtek:pdf-admincontenido

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
		stage('UML images'){
			agent any
            steps{
                sh '''
                    cd ./docker-plantuml
					chmod +x copy.sh
                    docker build . -t  "softtek:plantuml-admincontenido"
                    docker run --name plantuml_admincontenido -v v-rdl-admincontenido:/rdl/input/src-gen -v v-uml-admincontenido:/plantuml softtek:plantuml-admincontenido
                '''
            }
        }
		stage('Create PDF with Latex'){
			agent any
            steps{
                sh '''
                    cd ./docker_latex
					chmod +x copy.sh
                    docker build . -t  "softtek:latex-admincontenido"
                    docker run --name latex_admincontenido -v v-rdl-admincontenido:/rdl/input/src-gen -v v-screenshots-admincontenido:/cypress/screenshots -v v-uml-admincontenido:/plantuml -v v-pdf-admincontenido:/pdf softtek:latex-admincontenido
                '''
            }
        }
		stage('Publish PDF'){
			agent any
            steps{
                sh '''
					cd ./docker_pdf
					docker cp latex_admincontenido:/pdf/functional-spec.pdf ./
                    docker build . -t softtek:pdf-admincontenido
                    docker run --name pdf_admincontenido -d -p 172.16.68.31:4200:80 softtek:pdf-admincontenido
                '''
            }
        }
    }
}
