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
                powershell '''
                    cd ./docker_rdl
                    docker build . -t "softtek:rdl-admincontenido"
                    docker run --name rdl_admincontenido -v v-rdl-admincontenido:/rdl/input/src-gen softtek:rdl-admincontenido
                '''
            }
        }
    }
}
