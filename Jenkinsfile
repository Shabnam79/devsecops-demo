pipeline{
    agent any 
    tools{
        gradle 'Gradle'
        }
    stages
    {
	    stage('SCA'){
		    steps{
    dependencyCheckAnalyzer datadir: 'dependency-check-data', isFailOnErrorDisabled: true, hintsFile: '', includeCsvReports: false, includeHtmlReports: false, includeJsonReports: false, isAutoupdateDisabled: false, outdir: '', scanpath: '', skipOnScmChange: false, skipOnUpstreamChange: false, suppressionFile: '', zipExtensions: ''
 
    dependencyCheckPublisher canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
 
    archiveArtifacts allowEmptyArchive: true, artifacts: '**/dependency-check-report.xml', onlyIfSuccessful: true
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
