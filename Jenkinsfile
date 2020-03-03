pipeline {  	
    agent any
    stages{
	    def app
        stage('Cloning Git') {
            steps{
                /* Let's make sure we have the repository cloned to our workspace */
                checkout scm
            }            
        }  
    
        stage('Parallel SAST'){
            parallel{
                stage('SNYK SAST'){
                    steps{
			    echo 'Parallel SAST with SNYK'
			    build 'SAST-SNYK'
                    } 
                }
                stage('Sonar SAST'){
                    steps{
			    echo 'Parallel SAST with Sonar'
			    build 'Sonar-SAST'
                    }
                }
            }
        }
	    
	stage('Build-and-Tag') {
		steps{
			script{
				/* This builds the actual image; synonymous to
				* docker build on the command line */
				sh "echo Build-and-Tag stage"
				app = docker.build("orrin1001/snake")
			}
		}
	}
	    
		
    }
}
