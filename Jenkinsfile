node{
    stage('SCM CHECKOUT') { 
      git (credentialsId: 'git-cred', url: 'https://github.com/ravindrasinghh/spring3hibernate',branch: 'master')
      
    }
    stage('MVN Package'){
        sh 'mvn clean package'
    }
    stage('Build Docker image'){
        sh 'sudo docker build -t ravindrasingh6969/myapp .'
    }
    stage('Push Docker image'){
        withCredentials([string(credentialsId: 'docekrhub', variable: 'dockerhubpasswd')]) {
            sh "docker login -u ravindrasingh6969 -p ${dockerhubpasswd}"
        }
        sh 'sudo docker push ravindrasingh6969/myapp'
    }

    stage('Run on Dev server'){
        def dockerRun = 'docker run -itd -p 9092:8080 ravindrasingh6969/myapp'
        sshagent(['ec2-user']) {
            sh "ssh -o StrictHostKeyChecking=no root@ip ${dockerRun}"
       }
    }
}
