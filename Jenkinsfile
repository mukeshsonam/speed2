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
                script{
                  sh "docker build -t mukesh92/speed3:${BUILD_NUMBER} ."
               }
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
            withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8stoken', namespace: 'jenkins', restrictKubeConfigAccess: false, serverUrl: 'https://172.31.40.170:6443') {
            sh "kubectl apply -f speedk8s.yml"
            sh 'kubectl set image deployments/speed speed=mukesh92/speed3:${BUILD_NUMBER}'
              }
            }
        }
    }
}
