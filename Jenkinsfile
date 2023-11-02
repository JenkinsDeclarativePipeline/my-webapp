pipeline
{
    agent
    {
        label 'maven'
    }
    stages
    {
        stage('Docker Login')
        {
            agent
            {
                docker
                {
                    registryUrl 'https://hub.docker.com/'
                    registryCredentialsId 'docker_hub'
                }
            }
            steps
            {
                echo "Logged in"
            }
        }
    }
}