pipeline{
    agent any 
    tools{
        gradle 'Gradle'
        }
    stages
    {
       stage('Build')
        {
            steps 
            {
                    // for build
                    sh 'gradle clean build --no-daemon'                                        
            }
        }
       stage('git secret check')
        {
          steps
            {
	          script
               {
		           echo 'running trufflehog to check project history for secrets'
		           sh 'trufflehog --regex --entropy=False --max_depth=3 https://github.com/Shabnam79/devsecops-demo.git'
	           }
           }
      }
    }
}
