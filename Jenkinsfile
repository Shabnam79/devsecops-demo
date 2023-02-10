pipeline{
    agent any 
    tools{
        gradle 'Gradle'
        }
    stages
    {
	    stage('SCA'){
		    steps{
                   sh 'dependency-check.sh --scan . -f XML -o .'
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
