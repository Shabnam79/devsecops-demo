pipeline{
    agent any 
    tools{
        gradle 'Gradle'
        }
    stages
    {
       stage ('Software Composition Analysis'){
            steps {
              sh 'dependency-check.sh --scan . -f XML -o .'
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
