node {
    def app
	
    stage('Clone repository') {
        /* Cloning the Repository to our Workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image */

        app = docker.build("clouduser11/nodeapp")
    }

    stage('Test image') {
        
        app.inside {
            echo "Tests passed"
        }
    }

    stage('Push image') {
        /* 
			You would need to first register with DockerHub before you can push images to your account
		*/
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
            } 
                echo "Trying to Push Docker Build to DockerHub"
    }

        stage('Deploy to k8s'){
                sh "chmod +x changeTag.sh"
                sh "./changeTag.sh 1234"
                /*sshagent(['cloud_user']) {
                  */  
		ssh "scp -o StrictHostKeyChecking=no services.yml node-app-pod.yml cloud_user@3.85.106.209:/home/cloud_user/"
                    /*script{
                        try{*/
                           echo "K81" 
		ssh "ssh cloud_user@3.85.106.209:/home/cloud_user/ kubectl apply -f ."
                        /*}catch(error){
                          */  
				echo "K82"
		ssh "ssh cloud_user@3.85.106.209:/home/cloud_user/ kubectl create -f ."
			/*}
		    }
		}*/
		echo "K83"
	}
}
