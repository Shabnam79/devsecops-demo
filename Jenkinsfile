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
	stage('Check Git Secrets') {
              steps {
                    sh 'rm trufflehog || true'
                    sh 'docker run -t gesellix/trufflehog --json https://github.com/Shabnam79/devsecops-demo.git > output.txt'
		    sh "cat output.txt | grep -oE '\\\"stringsFound\\\":.[^\\\\\\\"]*{' | sed -e 's/,\\\\\\\".*//' -e 's/}//' | sed 's/\\\\\\\"stringsFound\\\\\\\"://' | grep -o '\\\"[^\\\\\\\"]*\\\"' | awk -F ',' '{ for(i=1;i<=NF;i++) print \$i}'"

   }
}
	 stage ('Docker File Scan'){
			steps{
				  sh 'checkov -f Dockerfile --skip-check CKV_DOCKER_3 '        //skip USER in Dockerfile with CKV_DOCKER_3
			}
		}

       stage('Build'){
            steps{
		 sh 'ls '
                 sh 'docker build -t devsecops . '
//                 sh 'docker tag devsecops:latest shabnam790/devsecops:latest'
//                  withDockerRegistry([credentialsId: "Dockerhub", url: ""]) 
//                  {
//                      sh  'docker push shabnam790/devsecops:latest'
//                    }
            }
        }
	  stage('Publish Docker Image to ECR'){
			steps{
				sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/r2c7u8u4'
				sh 'docker tag devsecops:latest public.ecr.aws/r2c7u8u4/devsecops:latest'
				sh 'docker push public.ecr.aws/r2c7u8u4/devsecops:latest'
			}
		}

	stage('Image Scanning')
	    {
		    steps{
			 //   sh 'wget https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/html.tpl'
			  //  sh 'trivy image --format template --template @./html.tpl -o report.html shabnam790/devsecops:latest'
			      sh 'trivy image shabnam790/devsecops:latest '
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
