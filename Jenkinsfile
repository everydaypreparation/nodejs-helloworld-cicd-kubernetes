pipeline{
    agent any
    stages {
        stage('Build Maven') {
            steps{
                git credentialsId: 'Prepare', url: 'https://github.com/everydaypreparation/nodejs-helloworld-cicd-kubernetes.git'
            }
        }
        stage('NPM Install') {
            steps {
                script {
                  sh 'npm install'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                  sh 'docker build -t prakashgolait/node-helloworld .'
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'prakashgolait', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u prakashgolait -p ${dockerhubpwd}'
                 }  
                 sh 'docker push prakashgolait/node-helloworld'
                }
            }
        }
    
    stage('Deploy App on k8s') {
      steps {
            script {
            kubernetesDeploy(configs: "deployment.yaml", kubeconfigId: "kubernetes")
            }
        }
    }
    }
    
}
