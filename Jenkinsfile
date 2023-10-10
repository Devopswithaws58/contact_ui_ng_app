pipeline {
    agent any
        tools {
            nodejs 'NodeJS-20.8.0'
            }

    stages {
        stage('git clone') {
            steps {
                git branch: 'main', 
                    credentialsId: 'GIT_CREDS', 
                    url: 'https://github.com/Devopswithaws58/contact_ui_ng_app.git'
            }
        }
        stage('NG Build') {
            steps {
                sh 'npm version'
                sh 'npm install'
                sh 'npm install -g @angular/cli@latest'
                sh 'ng version'
                sh 'ng build'
            }
        }
        stage('docker image') {
            steps {
               sh 'docker build -t devopswthaws58/contact_ui_app .' 
            }
        }
        stage('docker push'){
            steps {
                withCredentials([string(credentialsId: 'DOCKER-HUB-CREDENTIALS', variable: 'dockerpwd')]) {
                       sh 'docker login -u devopswthaws58 -p ${dockerpwd}'
                       sh 'docker push devopswthaws58/contact_ui_app'
                }
            }
        }
        stage('deply frontend to K8S'){
            steps {
                sh 'kubectl delete deployment contactuiappdeployment'
                sh 'kubectl apply -f contact_ui_deployment.yml'
            }
        }
        stage('trigger contcat_backend_job'){
            steps {
                build 'NG_Backend_Job'
            }
        }
    }
    post{
        always{
          emailext to: "devopswithaws58@gmail.com",
          subject: "jenkins build:${currentBuild.currentResult}: ${env.JOB_NAME}",
          body: "${currentBuild.currentResult}: Job ${env.JOB_NAME}\nMore Info can be found here: ${env.BUILD_URL}",
		      attachLog: true
        }
    }
    
}
