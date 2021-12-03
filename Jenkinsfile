pipeline {
    agent any
    environment {
        PROJECT_ID = 'vast-pride-329511'
		CLUSTER_NAME = 'new-cluster'
		LOCATION = 'us-central1-c'	
		CREDENTIALS_ID = 'Gke'
	
	}

    stages {
        stage('scm checkout') {
	    steps {
	         checkout scm
		 }
	}
        stage('git') {
            steps {
                git branch: 'main', url: 'https://github.com/jayalakshmi42/new-repo.git'
            }
        }
    
        stage('build docker image') {
            steps {
                script {
                    bat 'docker build -t jayalakshmi27/pipeline-new .'
                }
            }
        }
        stage('push docker image') {
            steps {
                script {
                   
                    bat'docker login -u "jayalakshmi27" -p "Lakshmi@427" docker.io' 
                }
                    bat 'docker push jayalakshmi27/pipeline-new'                   
            }
        }
        stage('Deploy on kube') {
            steps {
                
                   
                    step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME , location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: 'Gke', verifyDeployments: true])
                    step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME , location: env.LOCATION, manifestPattern: 'nginx-svc.yaml', credentialsId: 'Gke', verifyDeployments: true])
                
            }
        }
    }
}
