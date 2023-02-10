pipeline{
    agent any 
    tools{
        gradle 'Gradle'
        }
    stages
    {
	    stage('SCA'){
		    steps{
                  dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'OWASP-DC'

                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		    }
	    }
       stage('Build'){
            steps{
                    // for build
                    sh 'gradle clean build --no-daemon'                                        
            }
	}
    }
}
