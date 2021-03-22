def gv //define a global variable   groovy syntax
def item = hudson.model.Hudson.instance.getItem("my-pipeline") 
def build = item.getLastBuild()
def ver = build.getNumber()
pipeline {
    agent any

    //All environmental variables here in Jenkinsfile are available in the external groovy script
    parameters {
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: '')
        booleanParam(name: 'executeTests', defaultValue: true, description: '')
    }


    stages {
        stage("init"){
            steps{
                script { //groovy script 
                    gv = load "script.groovy"  //import external script
                    ver++
                    echo "${BUILD_NUMBER}"
                    echo "${ver}"
                }               
            }
        }
        stage("build"){
            steps{
                script { 
                    gv.buildApp()  //Using the function in external script
                }
            }
        }

        stage("test"){
            when {
                expression {
                    params.executeTests // or  params.executeTests == true
                }
            }
            steps{
                script {
                    gv.testApp()
                }
            }
        }

        stage("deploy"){
            steps{
                script {
                    gv.deployApp()
                }
            }
        }
    }
}
