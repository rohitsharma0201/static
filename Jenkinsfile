pipeline {
    agent any
    stages {
      stage('Lint HTML') {
        steps {
          sh 'tidy -q -e *.html'
         }
       }
      stage(‘AWSUploadS3’) {
        steps {
          withAWS(region:'ap-south-1',credentials:'aws-static') {
            s3Upload(pathStyleAccessEnabled:true, payloadSigningEnabled: true, file:'index.html', bucket:'udacity628')
          }
        }
      }
    }
}
