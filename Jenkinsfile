pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Maven3"
    }
    
    stages {
        stage('chekout') {
            steps {
                git branch: 'main', url: 'https://github.com/mukeshsonam/speed2.git'
            }
        }
        
        stage('build') {
            steps {
                sh "mvn clean verify"
            }
        }
        
        stage('docker_build') {
            steps {
             sh "docker build -t mukesh92/speed3:${BUILD_NUMBER} ."
            }
        }    
        stage('docker_push') {
            steps {
                withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dokcerhubpwd')]) {
                sh "docker login -umukesh92 -p ${dokcerhubpwd}"
                }
                sh "docker push mukesh92/speed3:${BUILD_NUMBER}"
            }
        }
        stage('Deployk8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'speedk8s.yml', kubeconfigId: 'k8scfg')
                    
                }
            }
        }
    }
}
