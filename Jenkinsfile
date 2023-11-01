pipeline
{
    agent
    {
        label 'maven'
    }
    environment
    {
        SONAR_CRED = credentials('sonar_secret')
    }
    stages
    {
        stage('SCM checkout')
        {
            steps
            {
                checkout scmGit(branches: [[name: '*/master']], 
                extensions: [], userRemoteConfigs: [[credentialsId: 'JenkinsDeclarativePipeline', 
                url: 'https://github.com/JenkinsDeclarativePipeline/my-webapp.git']])
            }            
        }
        stage('sonarQube Code Check')
        {
            steps
            {
                  sh '''  mvn sonar:sonar \\
                          -Dsonar.projectKey=maven-practice \\
                          -Dsonar.host.url=http://2.64.92.59:9000 \\
                          -Dsonar.login=${SONAR_CRED_PSW}'''
            }
        }
        stage('Maven Project Build')
        {
            tools
            {
                maven 'mvn-3.9.5'
            }
            steps
            {
                sh 'mvn clean package'
            }
        }
    }
}
