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
        build_number = getBuildNumber()
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
                          -Dsonar.host.url=http://3.25.201.232:9000/ \
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

                    docker.build("mywebapp:${build_number}")
                }
            }
            
        }
        stage('Docker Image Push')
        {
            agent
            {
                docker
                {
                    image "mywebapp:${build_number}"
                    registryUrl 'https://index.docker.io'
                    registryCredentialsId 'docker_hub'
                }
                
            }
            steps
            {
                sh "docker push mywebapp:${build_number}"
            }
        }
            
    }
}
def getBuildNumber()
{
    def buildNumber = currentBuild.number
    return buildNumber
}

 /*    // This step should not normally be used in your script. Consult the inline help for details.
                    withDockerRegistry(credentialsId: 'docker_hub', url: 'https://hub.docker.com/') 
                    {
                        
                                sh "docker push mywebapp:${build_number}"
                        
                    }*/
