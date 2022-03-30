def filepath1 = "C:/ProgramData/jenkins/.jenkins/workspace/dotnetappscm/aspnet-core-dotnet-core/aspnet-core-dotnet-core.csproj"//store source file path into a variable named filepath1
def file1 = "aspnet-core-dotnet-core/bin/Debug/netcoreapp1.1/publish" //published file
/*def jfrog path = "D:/jfrog/"*/ //Store downloaded artifact from jfrog stage.



pipeline 
{
   /* environment
    {
        appName = "sruthi-webapp"
        resourceGroup = "Training-rg"
        
    }*/
    agent any	// tells jenkins where to execute the code
	stages  //used to group the tasks
	{
        stage('sonarqube') //stage is for analysis of code
        {
            steps//it is used to group the step
            {
                script
                {
                    
                    def scannerHome = tool 'SonarScanner for MSBuild'//define variable to store the installed tool
                    withSonarQubeEnv('SonarQube Server') //block that allows you to select the SonarQube server you want to interact with
                    {
                        bat "\"${scannerHome}\\SonarScanner.MSBuild.exe\" begin /k:\"demo\""
                        
			    
			bat "dotnet build ${filepath1}"    
                        bat "\"${scannerHome}\\SonarScanner.MSBuild.exe\" end"
                    }
                }
            }
        }
        stage("Quality gate") // check the status
        {
            steps {
                waitForQualityGate abortPipeline: false
            }
        }
        stage('build')//Builds the project and its dependencies
        {
            steps
            {
	        echo "Builds the project and its dependencies"
                bat "dotnet build ${filepath1}"
		    
            }
        }
        stage('test') //The dotnet test command is used to execute unit tests in a given solution
        {
            steps
            {
                bat "dotnet test ${filepath1}"
		    
            }
        }
        stage('publish') // application is ready for deployment
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
                
		bat "tar.exe -a -c -f WebApp_${BUILD_NUMBER}.zip ${file1}"
		//packaged Zip file name with buildnumber
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
       /* stage('Upload file to jfrog')//This allows managing the File Specs in a source control
        {
            steps{
                rtUpload (
                 serverId:"Artifactory" ,//Creating an Artifactory Server Instance also in jenkins
                  spec: '''{
                   "files": [
                      {
                      "pattern": "${WORKSPACE}/WebApp_${BUILD_NUMBER}.zip",
                      "target": "sruthi-dotnetapp/"// repository in jfrog
                      }
                            ]
                           }''',
                        )
            }
        }/*
/*stage ('Publish build info') //url for the build
        {
            steps 
            {
                rtPublishBuildInfo (
                    serverId: "Artifactory"
                )
            }
        }*/
/*stage ('download the artifacts from artifactory') //artifacts are downloaded and moved to target path
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
                                    "target": "${jfrog path}"          
                                  }
                               ]
                              }"""
      )
            }
        }*/
	stage('Deploy to azure') 
	{
	   steps
		{
		   
			//azureWebAppPublish appName: "${env.appName}", azureCredentialsId: 'Azure', resourceGroup: "${env.resourceGroup}"
			azureWebAppPublish azureCredentialsId: params.Credentials_id , resourceGroup: params.ResourceGroup , appName: params.AppName
	    }
	}
	}
}

