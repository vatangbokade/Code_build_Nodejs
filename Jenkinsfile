pipeline {
  agent any
  tools {nodejs 'node'}
  stages {
        
    stage('Git') {
      steps {
        git branch: 'main', url: 'https://github.com/vatangbokade/Code_build_Nodejs.git'
      }
    }
     
    stage('install') {
      steps {
        sh 'npm install'
      }
    }  
    
            
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }
    stage('Build') {
      steps {
         sh 'npm run build'
      }
    } 
    

    
    stage('Build Docker image'){
          steps{
               
               sh 'docker build -t asia-south1-docker.pkg.dev/business-transformers/my-source-repo-suchita/nodejs-proj:${BUILD_NUMBER} .'
          }
      }
    
    stage('Docker push to Artifact Registry')
    {       steps{
                sh 'gcloud auth configure-docker asia-south1-docker.pkg.dev --quiet'
                sh 'docker push asia-south1-docker.pkg.dev/business-transformers/my-source-repo-suchita/nodejs-proj:${BUILD_NUMBER}'
           }
    
       }
      
      
        stage('Hello Print stage') {
         steps{
           sh 'echo hello world'
           }
         }
       stage('Deploy Production') {
            steps{
                 git branch: 'main', url: 'https://github.com/vatangbokade/Code_build_Nodejs.git'
                step([$class: 'KubernetesEngineBuilder', 
                        projectId: "business-transformers",
                        clusterName: "cluster-suchita-private",
                        zone: "us-central1-a",
                        manifestPattern: 'k8s/',
                        credentialsId: "business-transformers",
                        verifyDeployments: true])
            }
        } 
    
    
}
}
