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
					LOG_FILE=\'log.$(date +"%Y%m%d_%H%M%S")\'
					docker stop prototipo_admincontenido &>> ./logs/$LOG_FILE
					docker rm rdl_admincontenido &>> ./logs/$LOG_FILE
					docker rm prototipo_admincontenido &>> ./logs/$LOG_FILE
					docker rm screenshots_admincontenido &>> ./logs/$LOG_FILE
					docker rm plantuml_admincontenido &>> ./logs/$LOG_FILE
					docker rm latex_admincontenido &>> ./logs/$LOG_FILE
					
					docker rmi softtek:rdl-admincontenido &>> ./logs/$LOG_FILE
					docker rmi softtek:riot-admincontenido &>> ./logs/$LOG_FILE
					docker rmi softtek:screenshots-admincontenido &>> ./logs/$LOG_FILE
					docker rmi softtek:plantuml-admincontenido &>> ./logs/$LOG_FILE
					docker rmi softtek:latex-admincontenido &>> ./logs/$LOG_FILE
					docker rmi softtek:pdf-admincontenido &>> ./logs/$LOG_FILE
					
					docker volume rm v-rdl-admincontenido &>> ./logs/$LOG_FILE
					docker volume rm v-screenshots-admincontenido &>> ./logs/$LOG_FILE
					docker volume rm v-uml-admincontenido &>> ./logs/$LOG_FILE
					docker volume rm v-pdf-admincontenido &>> ./logs/$LOG_FILE
					
					docker volume create --name v-rdl-admincontenido
					docker volume create --name v-screenshots-admincontenido
					docker volume create --name v-uml-admincontenido
					docker volume create --name v-pdf-admincontenido
				'''
			}
		}
    }
}
