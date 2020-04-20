options(
    [
        pipelineTriggers(
            [
                githubPush()
            ]
        )
    ]
)
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
                withAWS(region: 'eu-west-2', credentials: 'aws-static') {
                    sh 'echo "Uploading content with AWS creds"'
                    s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file: 'index.html', bucket: 'hellovans-files', acl: 'PublicRead')
                }
            }
        }
        stage('Check content is available') {
            steps {
                sh 'curl -sf "https://hellovans-files.s3.eu-west-2.amazonaws.com/index.html" >/dev/null'
            }
        }
    }
}