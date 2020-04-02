properties([pipelineTriggers([githubPush()])])

pipeline {
    agent any
    stages {
        stage('Lint HTML') {
            steps {
                sh 'tidy -q -e *.html'
            }
        }
        stage('Upload to AWS') {
            steps {
                withAWS(region:'ap-south-1',credentials: 'aws-static') {
                    timeout(time: 3, unit: 'MINUTES') {
                        retry(5) {
                            echo 'Uploading content with AWS creds'
                            s3Upload(pathStyleAccessEnabled:true, payloadSigningEnabled: true, file:'index.html', bucket:'udacity6028')
                        }
                    }
                }
            }
        }
        stage('Post Deploy Test') {
            steps {
                echo 'Testing Deployment'
                script {
                    String webSite = 'https://udacity6028.s3-website.ap-south-1.amazonaws.com//index.html'
                    def returnCode = sh(returnStdout: true, script: "curl -sLI -o /dev/null -w '%{http_code}' ${webSite}").trim() as Integer
                    if (returnCode != 200) {
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }
    }
    /* Cleanup workspace */
    post {
       always {
           deleteDir()
       }
    }
}
