node ('agent1'){  
    def app
    
    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
       checkout scm
    }  
    /*stage('SAST'){
        build 'SECURITY-SAST-SNYK'
    }*/

    
    stage('Build-and-Tag') {
    /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build("fivebird/snake:latest")
    }
    
    stage('Post-to-dockerhub') {
    
     docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_cred') {
            app.push("latest")
        			}
    }
    /*stage('SECURITY-IMAGE-SCANNER'){
        build 'SECURITY-IMAGE-SCANNER-AQUAMICROSCANNER'
    }*/
  
    
    stage('Pull-image-server') {
         sh "docker-compose restart node-multiplayer-snake_snake_1"
         //sh "docker-compose up -d"	
    }
    
    /*stage('DAST')
        {
        build 'SECURITY-DAST-OWASP_ZAP'
        }*/
 
}
