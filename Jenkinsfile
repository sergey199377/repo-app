pipeline {
    agent any
    options { timestamps () }
    environment {
        MYAPP_CI_TOKEN = credentials('MYAPP_CI_TOKEN')
        K8S_MYAPP_AUTH = 'kube-api-myapp.ru'
        REGISTRY_NAME = myapp-repo
        AWS_ACCOUNT_ID = credentials('AWS_ACCOUNT_ID')
        
 
        failure = '%F0%9F%9A%AB'
        success = '%E2%9C%85'
    }

    stages {
      
      stage('Setup kubeconfig') {
        
        steps {
          
            sh """
            kubectl config set-cluster k8s --insecure-skip-tls-verify=true --server=https://${APISERVER_ADDR}
            kubectl config set-credentials ci --token=${TOKEN}
            kubectl config set-context k8s --cluster=k8s --user=ci
            kubectl config use-context k8s
          """

          }
        }
     
      stage('deploy') {
 
           sh """
             docker build -t ${AWS_ACCOUNT_ID}.dkr.ecr.eu-central-1.amazonaws.com/myapp:latest
             docker push ${AWS_ACCOUNT_ID}.dkr.ecr.eu-central-1.amazonaws.com/myapp:latest
             
             helm install jenkins-deploy-myapp -f ./helm-myapp/values.yaml ./helm-myapp/.             

       }
   }

}
