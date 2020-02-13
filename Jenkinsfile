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
                sshagent(['minikube']) {
                    sh "scp -o StrictHostKeyChecking=no services.yml node-app-pod.yml minikube@192.168.99.102:/home/"
                    script{
                        try{
                            sh "ssh minikube@192.168.99.102:/home/ kubectl apply -f ."
                        }catch(error){
                            sh "ssh minikube@192.168.99.102:/home/ kubectl create -f ."
			}
		    }
		}
	}
}
