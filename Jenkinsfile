@Library('my-shared-library') _

pipeline{
    agent any
    parameters{
        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/destroy')
    }
    stages{
        stage('Git Checkout'){
        when {expression{parms.action == 'create'}}
            steps{
            gitCheckout(
                branch: "main",
                url: "https://github.com/vikash-kumar01/mrdevops_java_app.git"
            )
            }
        }
        stage('Unit Test maven'){
        when{expression{parms.action == 'create'}}
            steps{
               script{
                   mvnTest()
               }
            }
        }
        stage('Integration Test maven'){
        when {expression{parms.action == 'create'}}
            steps{
               script{
                   mvnIntegrationTest()
               }
            }
        }
        stage('Static code analysis: sonarqube'){
        when {expression{parms.action == 'create'}}
            steps{
               script{
                   statiCodeAnalysis()
               }
            }
        }       
    }
}
