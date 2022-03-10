node{
   stage('Fetch from Github'){
     git 'https://github.com/ramksures/my-app.git'
   }
   stage('PackageCompiling'){

      def mvnHome =  tool name: 'warfileCre', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }

 stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'warfileCre', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }

stage('Build Docker Imager'){
   sh 'docker build -t ramksures/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u ramksures -p ${dockerPassword}"
    }
   sh 'docker push ramksures/myweb:0.0.2'
   }
 stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest ramksures/myweb:0.0.2' 
   }
}

}

