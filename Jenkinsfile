def filepath1 = "C:/ProgramData/jenkins/.jenkins/workspace/dotnetappscm/aspnet-core-dotnet-core/aspnet-core-dotnet-core.csproj"


pipeline 
{
    environment
    {
        appName = "sruthi-webapp"
        resourceGroup = "Training-rg"
        
    }
    agent any	
	stages 
	{
        stage('sonarqube') 
        {
            steps
            {
                script
                {
                    
                    def scannerHome = tool 'SonarScanner for MSBuild'
                    withSonarQubeEnv('SonarQube Server') 
                    {
                        bat "\"${scannerHome}\\SonarScanner.MSBuild.exe\" begin /k:\"demo\""
                        
			    
			bat "dotnet build ${filepath1}"    
                        bat "\"${scannerHome}\\SonarScanner.MSBuild.exe\" end"
                    }
                }
            }
        }
        /*stage("Quality gate") 
        {
            steps {
                waitForQualityGate abortPipeline: false, credentialsId: 'squser'
            }
        }*/
        stage('build')
        {
            steps
            {
                bat "dotnet build ${filepath1}"
		    
            }
        }
        stage('test')
        {
            steps
            {
                bat "dotnet test ${filepath1}"
		    
            }
        }
        stage('publish')
        {
            steps
            {
                bat "dotnet publish ${filepath1}"
		   
            }
        }
        
      /*  stage('Package') 
        {
            steps 
                {
                echo "Deploying to stage environment for more tests!";
                bat "del *.zip"
                
		bat "tar.exe -a -c -f WebApp_${BUILD_NUMBER}.zip aspnet-core-dotnet-core/bin/Debug/netcoreapp1.1/publish"
                }
        }*/
        
       /* stage ('Connected to jfrog')
        {
            steps
            {
               rtServer (
                 id: "Artifactory",
                 
                  bypassProxy: true,
                   timeout: 300
                        )
            }
        }*/
       /* stage('Upload file to jfrog')
        {
            steps{
                rtUpload (
                 serverId:"Artifactory" ,
                  spec: '''{
                   "files": [
                      {
                      "pattern": "*.zip",
                      "target": "sruthi-dotnetapp/"
                      }
                            ]
                           }''',
                        )
            }
        }/*
/*stage ('Publish build info') 
        {
            steps 
            {
                rtPublishBuildInfo (
                    serverId: "Artifactory"
                )
            }
        }*/
/*stage ('download the artifacts from artifactory')
        {
            steps
            {
                  rtDownload (
                    serverId: "Artifactory",
                        spec:
                              """{
                                "files": [
                                  {
                                    "pattern": "sruthi-dotnetapp/*.zip",
                                    "target": "D:/jfrog/"          
                                  }
                               ]
                              }"""
      )
            }
        }*/
   /* stage('Extract ZIP') 
	{
	   steps
	   {
		   powershell '''
		                  Expand-Archive 'D:/jfrog/dotnetapp.zip' -DestinationPath 'D:/home/'
		              '''
        } 
	}*/
	stage('Deploy to azure') 
	{
	   steps
		{
		   
			azureWebAppPublish appName: "${env.appName}", azureCredentialsId: 'Azure', resourceGroup: "${env.resourceGroup}"
	    }
	}
	}
}

