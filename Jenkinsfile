pipeline{
    agent any 
    tools{
        gradle 'Gradle'
        }
    stages
    {
       stage('Dependency Checking'){
		steps{
			script{
				
				dependencyCheck additionalArguments: '--format XML', odcInstallation: 'SCA'
				dependencyCheckPublisher pattern: ''
			}
		}
	}
       stage('Build'){
            steps{
		 sh 'ls '
                 sh 'docker build -t devsecops . '
                 sh 'docker tag devsecops:latest pritidevops/devsecops:latest'
                 withDockerRegistry([credentialsId: "Dockerhub", url: ""]) 
                 {
                     sh  'docker push pritidevops/devsecops:latest'
                   }
            }
        }

    }
}
