pipeline{
    agent{
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "devops-jenkins-ci-cd"
    }
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/sbalavardhan/GitOps-DevOps-Jenkins-CI-CD.git'
            }
       }
       stage("update the deployment tags"){
           steps{
            sh """
               cat deployment.yaml
               sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
               cat deployment.yaml
               """
           }
       }
       stage("push the changes to github repo gitops"){
        steps{
         sh """
            git config --global user.name "jenkins"
            git config --global user.email "jenkins@gmail.com" 
            git add deployment.yaml # git add . also fine
            git commit -m "updated deployment manifest"
            """ 
            withCredentials([gitUsernamePassword (credentialsId: 'github',gitToolName: 'Default')]){
                sh 'git push origin main'
            } 
        }
       }
    }
}
