pipeline
{
    agent
    {
        label 'maven'
    }
    tools
    {
        maven 'mvn-3.9.5'
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
                          -Dsonar.host.url=http://52.64.92.59:9000/ \\
                          -Dsonar.login=sqp_a39cde1e37b3368b3ce32873897575263deaed4a'''
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
        stage('Docker Build')
        {
            steps
            {
                script
                {
                    docker.build('mywebapp:0.0.1','-t indrajith/myapp:0.1')
                }
            }
            
        }
    }
}
