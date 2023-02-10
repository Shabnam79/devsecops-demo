pipeline{
    agent any 
    tools{
        gradle 'Gradle'
        }
    stages
    {
	    stage('SCA'){
		    steps{
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
