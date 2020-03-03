pipeline {  	
    agent{
	    node { label 'AppServer'} 
	    }
    stages{
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
	    
	stage('Build-Tag-Push') {
		steps{
			script{
				/* This builds the actual image; synonymous to
				* docker build on the command line */
				def app
				sh "echo Build-and-Tag stage"
				app = docker.build("orrin1001/snake")
				sh "echo Post-to-dockerhub stage"
				docker.withRegistry('https://registry.hub.docker.com','docker_cred') {
					app.push("latest")
				}
			}
		}
	}
		
		
            stage('Aqua Image Scanner'){
		    steps{
			    sh "echo Image Scanner with Aqua stage"
                		build 'ImageScanner-Aqua'
		    }                
            }
        
  
    stage('Pull-image-server') {    
	    steps{
		    sh "echo Pull-image-server stage"
        		sh "npm install"
        		sh "docker-compose down"
        		sh "docker-compose up -d"	
        		sh "echo Delete all the none tag images"
        		sh '''
            		img_id=`docker images -q -f dangling=true`
            		docker rmi $img_id
        		'''
	    }
      }
	    
		
    }
}
