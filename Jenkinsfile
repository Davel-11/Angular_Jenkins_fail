node {
    def to = emailextrecipients([
      [$class: 'cdaltan']
    ])    
    def commit_id
      try
      {     
        stage('get data from gitt ')
        {
             checkout scm
             sh "git rev-parse --short HEAD > .git/commit-id"                        
             commit_id = readFile('.git/commit-id').trim()
        }
        stage('docker build/push') 
        {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') 
          {
            def app = docker.build("11davel/001-angular-jenkins01:${commit_id}", '.').push()
          }
        }                     
      } catch (e){
          currentBuild.result = "FAILURE"; 
          def subject = "${env.JOB_NAME} - Build #${env.BUILD_NUMBER} ${currentBuild.result}"
          def content = '${JELLY_SCRIPT,template="html"}'
        
          if(to != null && !to.isEmpty()) {
              emailext(body: content, mimeType: 'text/html',
              replyTo: '$DEFAULT_REPLYTO', subject: subject,
              to: to, attachLog: true )
          }
        
          throw e;
    }  
}
