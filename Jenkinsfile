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
					docker stop prototipo_admincontenido > /dev/null 2>&1
					docker rm rdl_admincontenido > /dev/null 2>&1
					docker rm prototipo_admincontenido > /dev/null 2>&1
					docker rm screenshots_admincontenido > /dev/null 2>&1
					docker rm plantuml_admincontenido > /dev/null 2>&1
					docker rm latex_admincontenido > /dev/null 2>&1
					
					docker rmi softtek:rdl-admincontenido > /dev/null 2>&1
					docker rmi softtek:riot-admincontenido > /dev/null 2>&1
					docker rmi softtek:screenshots-admincontenido > /dev/null 2>&1
					docker rmi softtek:plantuml-admincontenido > /dev/null 2>&1
					docker rmi softtek:latex-admincontenido > /dev/null 2>&1
					docker rmi softtek:pdf-admincontenido > /dev/null 2>&1
					
					docker volume rm v-rdl-admincontenido > /dev/null 2>&1
					docker volume rm v-screenshots-admincontenido > /dev/null 2>&1
					docker volume rm v-uml-admincontenido > /dev/null 2>&1
					docker volume rm v-pdf-admincontenido > /dev/null 2>&1
					
					docker volume create --name v-rdl-admincontenido
					docker volume create --name v-screenshots-admincontenido
					docker volume create --name v-uml-admincontenido
					docker volume create --name v-pdf-admincontenido
				'''
			}
		}
    }
}
