pipeline{
    agent any
    stages{
        stage('Build'){
            steps{
                sh 'echo "Hello World"'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
            }
        }
    }
        stage('Uploading to AWS') {
            steps{
                withAWS(region: 'ap-south-1', credentials: 'aws-static') {
                sh 'echo "Uploading content with AWS creds"'
                s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file: 'index.html', bucket: 'udacity6028')
            }
        }
    }
}
