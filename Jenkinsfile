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
                 sh 'docker tag devsecops:latest shabnam790/devsecops:latest'
                 withDockerRegistry([credentialsId: "Dockerhub", url: ""]) 
                 {
                     sh  'docker push shabnam790/devsecops:latest'
                   }
            }
        }
        stage ('Staging') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'docker-staging', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'docker rm -f devsecops-test && docker pull shabnam790/devsecops && docker run -itd -p 9090:9090 --name devsecops-test shabnam790/devsecops:latest', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: 'vulnerable-staging', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])            
            }
        }
	 stage ('Production') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'docker-production', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'docker rm -f devsecops-prod && docker pull shabnam790/devsecops && docker run -itd -p 9090:9090 --name devsecops-prod shabnam790/devsecops:latest --name devsecops-prod', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: 'vulnerable-prod', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])            
            }
        } 


    }
}
