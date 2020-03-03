node ('DevSecOps_App'){  
    def app
    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
       checkout scm
    }  
    
    stage('SNYK SAST'){
		sh "echo SAST with SNYK stage"
        build 'SAST-SNYK'
    }
    /*stage('Sonar SAST'){
        sh "echo SAST with Sonar stage"
        build 'Sonar-SAST'
    }*/
    
    
    
    stage('Build-and-Tag') {
    /* This builds the actual image; synonymous to
         * docker build on the command line */
        sh "echo Build-and-Tag stage"
        app = docker.build("orrin1001/snake")
    }
    
    stage('Post-to-dockerhub') {    
        sh "echo Post-to-dockerhub stage"
        docker.withRegistry('https://registry.hub.docker.com','docker_cred') {
            app.push("latest")
        }
    }
    
    stage('Aqua Image Scanner'){
        sh "echo Image Scanner with Aqua stage"
        build 'ImageScanner-Aqua'
    }
    /*stage('Achore Image Scanner'){
        sh "echo Image Scanner with Achore stage"
        build 'ImageScanner-Achore'
    }*/ 
  
    stage('Pull-image-server') {    
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
    
    stage('OWASP ZAP DAST'){
        sh "echo DAST with OWASP ZAP stage" 
        build 'DAST-OWASP-ZAP'
    }
    stage('Arachni DAST'){
        sh "echo DAST with Arachni stage" 
        build 'DAST-Arachni'
    }
}
