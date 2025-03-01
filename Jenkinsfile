@Library('my-shared-library') _

pipeline{

   agent any
  // agent {
  //      label 'java_node' // All stages will run on the agent with label 'my-agent'
   // test
   //test
  //  }
    parameters{

        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/Destroy')
        string(name: 'appname', description: "name of the docker app", defaultValue: 'javaapp')
        string(name: 'acrurl', description: "name of the docker build", defaultValue: 'javaappacr.azurecr.io')
        string(name: 'ImageTag', description: "tag of the docker build", defaultValue: 'v1')
    }

    stages{
         
        stage('Git Checkout'){
                    when { expression {  params.action == 'create' } }
            steps{
            gitCheckout(
                branch: "main",
                url: "https://github.com/vikash-kumar01/mrdevops_java_app.git"
            )
            }
        }
         stage('Unit Test maven'){
         
         when { expression {  params.action == 'create' } }

            steps{
               script{
                   
                   mvnTest()
               }
            }
        }
         stage('Integration Test maven'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   mvnIntegrationTest()
               }
            }
        }
        stage('Static code analysis: Sonarqube'){
         when { expression {  params.action == 'delete' } }
            steps{
               script{
                   
                   def SonarQubecredentialsId = 'sonarqube-api'
                   statiCodeAnalysis(SonarQubecredentialsId)
               }
            }
        }
        stage('Quality Gate Status Check : Sonarqube'){
         when { expression {  params.action == 'delete' } }
            steps{
               script{
                   
                   def SonarQubecredentialsId = 'sonarqube-api'
                   QualityGateStatus(SonarQubecredentialsId)
               }
            }
        }
        stage('Maven Build : maven'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   mvnBuild()
               }
            }
        }
        stage('Docker Image Build'){
         when { expression {  params.action == 'delete' } }
            steps{
               script{
                   
                   dockerBuild("${params.appname}","${params.acrurl}","${params.ImageTag}")
               }
            }
        }
         stage('Docker Image Scan: trivy '){
         when { expression {  params.action == 'delete' } }
            steps{
               script{
                   
                   dockerImageScan("${params.appname}","${params.acrurl}","${params.ImageTag}")
               }
            }
        }
        stage('Docker Image Push : DockerHub '){
         when { expression {  params.action == 'delete' } }
            steps{
               script{
                   
                   dockerImagePush("${params.appname}","${params.acrurl}","${params.ImageTag}")
               }
            }
        }   
        stage('Docker Image Cleanup : DockerHub '){
        when { expression {  params.action == 'delete' } }
         steps{
          script{
                   
             dockerImagePush("${params.appname}","${params.acrurl}","${params.ImageTag}")
            }
           }
       }      
    }
}
