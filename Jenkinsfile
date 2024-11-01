node('ubuntu-appserver-CWEB2140')
{
 
def app
stage('Cloning Git')
{
    /* Let's make sure we have the repository cloned to our workspace */
    checkout scm
}

stage('SCA-SAST-NodeJS-Chat-App-Testing')
{
    snykSecurity failOnIssues: false, snykInstallation: 'Snyk', snykTokenId: 'snyk_api_token'
}

stage('Build-and-Tag')
{
    /* This builds the actual image;
         * This is synonymous to docker build on the command line */
    app = docker.build('spcasagrande/nodejschatapp')
}
 
stage('Post-to-dockerhub')
{
    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_credentials')
    {
        app.push('latest')
    }
   
}
 
stage('Deploy')
{
    sh "docker-compose down"
    sh "docker-compose up -d"
}
 
}