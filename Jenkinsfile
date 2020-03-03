pipeline {  
    agent any
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
		
    }
}
