node ('maven')
{
    tool name: 'mvn-3.9.5', type: 'maven'

    stage('SCM Checkout')
    {
        checkout scmGit(branches: [[name: '*/master']], 
        extensions: [], userRemoteConfigs: [[credentialsId: 'JenkinsDeclarativePipeline', url: 'https://github.com/JenkinsDeclarativePipeline/my-webapp.git']])
    }
    stage('Maven Build')
    {
        sh '${mvn-3.9.5}/bin/mvn clean package'
    }
}