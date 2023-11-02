node ('maven')
{
    stage('SCM Checkout')
    {
        checkout scmGit(branches: [[name: '*/master']], 
        extensions: [], userRemoteConfigs: [[credentialsId: 'JenkinsDeclarativePipeline', url: 'https://github.com/JenkinsDeclarativePipeline/my-webapp.git']])
    }
}