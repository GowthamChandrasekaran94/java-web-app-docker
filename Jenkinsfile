node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/MithunTechnologiesDevOps/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.6.3", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t gowthamchandrasekaran/java-web-app .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'docker', variable: 'Docker_Hub_Pwd')]) {
          sh "docker login -u gowthamchandrasekaran -p ${Docker_Hub_Pwd}"
        }
        sh 'docker push gowthamchandrasekaran/java-web-app'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app gowthamchandrasekaran/java-web-app'
         
         sshagent(['ssh_connection']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.36.131 docker stop java-web-app || true'
          sh 'ssh  ubuntu@172.31.36.131 docker rm java-web-app || true'
          sh 'ssh  ubuntu@172.31.36.131 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.36.131 ${dockerRun}"
       }
       
    }
     
     
}
