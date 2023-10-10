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
            sh 'npm version'
        }
        stage('NG Build'){
            steps {
                sh 'npm version'
                sh 'npm install'
                sh 'npm install -g @angular/cli'
                sh 'ng verison'
                sh 'ng build'
            }
        }
    }
}
