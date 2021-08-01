node ('agent1'){  
    def app
    
    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
       checkout scm
    }  
    /*stage('SAST'){
        build 'SECURITY-SAST-SNYK'
    }*/
    stage('Start') {
            agent any
            steps {
                slackSend (channel: '#jenkins', color: '#FFFF00', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
            }
    }
    
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
        sh "docker-compose -f ~/app/node-multiplayer-snake/docker-compose.yml up -d"	
    }
    
    /*stage('DAST')
        {
        build 'SECURITY-DAST-OWASP_ZAP'
        }*/
    
    post {
        success {
            slackSend (channel: '#jenkins', color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        failure {
            slackSend (channel: '#jenkins', color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
    }
 
}
