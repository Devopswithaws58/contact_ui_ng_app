pipeline{
    
    agent any
    
    stages
    {
        stage('clone repo')
        {
            steps
            {
                git branch: 'development', 
                credentialsId: 'git-hub-creds', 
                url: 'https://github.com/Devopswithaws58/maven-web-app.git'
            }
        }
        stage('build and code review ')
        {
            steps
            {
                withSonarQubeEnv('sonar-server-9.9.2') 
                {
                    sh 'mvn clean package sonar:sonar'
                }
            }
        }
        /*stage('code review'){
            steps
            {
                withSonarQubeEnv('sonar-server-9.9.2') 
                {
                    sh 'mvn sonar:sonar'
                }
            }
            
        }*/
        stage('upload artifact')
        {
            steps
            {
                nexusArtifactUploader artifacts: 
                [
                    [
                        artifactId: '01-maven-web-app', 
                        classifier: '', 
                        file: 'target/01-maven-web-app.war', 
                        type: 'war'
                    ]
                ], 
                credentialsId: 'nexus-repo-credss', 
                groupId: 'in.ashokit', 
                nexusUrl: '43.205.231.109:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'devops-snapshot-repo', 
                version: '3.0-SNAPSHOT'
            }
        }
        stage('create and push image')
        {
            steps
            {
                sh 'docker build -t devopswthaws58/maven-web-app .'
                withCredentials([string(credentialsId: 'DOCKER-HUB-CREDENTIALS', variable: 'dockerpwd')]) 
                {
                    sh 'docker login -u devopswthaws58 -p ${dockerpwd}'
                    sh 'docker push devopswthaws58/maven-web-app'
                }
            }
        }
        /*stage('push image'){
            steps{
                withCredentials([string(credentialsId: 'DOCKER-HUB-CREDENTIALS', variable: 'dockerpwd')]) {
                    sh 'docker login -u devopswthaws58 -p ${dockerpwd}'
                    sh 'docker push devopswthaws58/maven-web-app'
                }
            }
        }*/
        stage('deploy on K8S')
        {
            steps
            {
                sh 'kubectl delete deploy mavenwebappdeployment'
                sh 'kubectl apply -f maven-web-app-deploy.yml'
            }
        }
    }
