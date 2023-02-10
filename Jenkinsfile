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
                    sh './gradlew clean build --no-daemon'                                        
            }
        }
    }
}
