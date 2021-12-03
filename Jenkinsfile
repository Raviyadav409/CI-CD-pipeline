pipeline {
    agent any
    
    environment {
   
         PROJECT_ID =  'valid-expanse-333605'
         CLUSTER_NAME = 'newcluster'
         LOCATION =  'us-east1-c'	
         CREDENTIALS_ID = 'Gke'
       }
       
    stages {
    
      stage('scm checkout') {
           steps {
        checkout scm 
           }
        }
          
        stage('git repo & clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Raviyadav409/CI-CD-pipeline.git'
            }
        }
        
        stage('Directory checkup') {
        steps {
            bat 'dir'
          }
        }
        
        stage('Docker build') {
            steps{
                bat 'docker build -t 1104ravi409/newimage:latest .'
               }
        }
        stage('Docker Push to dockerhub') {
            steps{
                    bat 'docker login -u "1104ravi409" -p "Raviyadav@1104" docker.io'

                    bat 'docker push 1104ravi409/newimage:latest'
                
               }
        }
        stage('Deploy on k8s') {
            steps{
                   step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME , location: env.LOCATION, manifestPattern: 'deploy.yaml', credentialsId: 'Gke', verifyDeployments: true])

                   step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME , location: env.LOCATION, manifestPattern: 'svc.yaml', credentialsId: 'Gke', verifyDeployments: true]) 
                    
               }
        }
        
        
    }
}
