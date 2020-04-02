pipeline {
  agent any 
  stages {
    stage('Build') {
        steps {
            sh 'echo "Hello World"'
            sh '''
            echo "Multiline shell steps works too"
            ls -lah'''
        }
    }
    stage('Lint HTML') {
        steps {
            sh 'tidy -q -e *.html'
        }
    }
    stage('Upload to AWS') {
        steps {
          timeout(time: 1, unit: 'MINUTES') {
            retry(2) {
              withAWS(region:'ap-south-1',credentials:'	aws-static') {
              s3Upload(pathStyleAccessEnabled:true, payloadSigningEnabled: true, file:'index.html', bucket:'udacity6028')
            }
          }
        }
      }
    }
    stage('Check Availabilty') {
        steps {
          sh 'curl -Is http://udacity6028.s3-website.ap-south-1.amazonaws.com | head -1'
        }
    }
  }
}
