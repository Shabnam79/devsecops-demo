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
       stage('Build')
       {
            steps
            {
                    // for build
                    sh 'gradle clean build --no-daemon'                                        
            }
	}
    }
}
