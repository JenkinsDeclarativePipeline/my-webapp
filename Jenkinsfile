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

        DOCKER_TAG = "0.1.2"
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
                    withCredentials([string(credentialsId: 'sonar_secret', variable: 'sonar_cred')]) 
                {
                    sh    "mvn sonar:sonar \
                          -Dsonar.projectKey=maven-practice \
                          -Dsonar.host.url=http://52.64.92.59:9000/ \
                          -Dsonar.login=${sonar_cred}"
                }
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
                    def build_number = currentBuild.number

                    docker.build("mywebapp:${build_number}")
                }
            }
            
        }
        state('Docker Image Push')
        {
            steps
            {
                script
                {
                    // This step should not normally be used in your script. Consult the inline help for details.
                    withDockerRegistry(credentialsId: 'docker_hub', url: 'https://hub.docker.com/') 
                    {
                        docker.image.push("mywebapp:${build_number}")
                    }
                }
            }
        }
    }
}
