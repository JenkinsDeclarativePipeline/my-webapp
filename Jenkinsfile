node ('maven')
{
    def mvn = tool name: 'mvn-3.9.5', type: 'maven'
    def build_number = currentBuild.number
    stage('SCM Checkout')
    {
        checkout scmGit(branches: [[name: '*/master']], 
        extensions: [], userRemoteConfigs: [[credentialsId: 'JenkinsDeclarativePipeline', url: 'https://github.com/JenkinsDeclarativePipeline/my-webapp.git']])
    }
    stage('Maven Build')
    {
        sh "${mvn}/bin/mvn clean package"
    }
    stage('Sonar Quality Check')
    {
        withCredentials([string(credentialsId: 'sonar_secret', variable: 'sonar_cred')])
        {
                    sh    "${mvn}/bin/mvn sonar:sonar \
                          -Dsonar.projectKey=maven-practice \
                          -Dsonar.host.url=http://3.25.201.232:9000/ \
                          -Dsonar.login=${sonar_cred}"
        }
    }
    stage('Docker Build')
    {
        //def build_number = currentBuild.number
        docker.build("mywebapp:${build_number}")
    }
    stage('Docker Build')
    {
        withCredentials([string(credentialsId: 'dockerhub_secret', variable: 'dockerhub_secret')]) 
        {
            sh "docker login -u uriyapraba -p ${dockerhub_secret}"
        }
        //def build_number = currentBuild.number
        docker.push("mywebapp:${build_number}")
    }
}