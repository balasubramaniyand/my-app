node{
   stage('SCM Checkout'){
     git 'https://github.com/balasubramaniyand/my-app'
   }
    stage('maven-builds'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"                
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('Nexus Image Push'){
   withCredentials([string(credentialsId: 'nexus', variable: 'nexusPassword')]) {
   sh "docker login -u admin -p ${nexuspassword} 54.151.241.253:8083"
    }
	sh "docker tag balasubramaniyand/myweb:0.0.2 54.151.241.253:8083/priyaa:0.0.1"
   sh 'docker push 54.151.241.253:8083/priyaa:0.0.1' 
    }
		   
    stage('Build Docker Image'){
   sh 'docker build -t balasubramaniyand/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u balasubramaniyand -p ${dockerPassword}"
    }
   sh 'docker push balasubramaniyand/myweb:0.0.2'  	   
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin1234 13.212.221.12:8083"
   sh "docker tag balasubramaniyand/myweb:0.0.2 13.212.221.12:8083/damo:1.0.0"
   sh 'docker push 13.212.221.12:8083/damo:1.0.0'
   }
    stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
    stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest balasubramaniyand/myweb:0.0.2' 
   }	    
}
}
