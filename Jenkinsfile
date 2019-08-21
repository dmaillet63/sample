#!/usr/bin/env groovy

//*************************************************************************
// File : Jenkinsfile
//
// Author(s) : David Maillet
//
// Notes: Uses Scripted Pipeline syntax
//
// References:
// Available Global Environment variables are listed in the running Jenkinsfile
// http://localhost:8080/pipeline-syntax/globals
//
//**************************************************************************

node {
    stage('build') {
        sh 'java -version'
    }
    parallel(
       'Build service 1': {
          sh 'java -version'
       },
       'Build service 2': {
          sh 'uname -a'
       }
    )
    stage('qa') {
        if (env.BRANCH_NAME == 'master') {
            echo 'QA step for the master branch'
        } else {
            echo 'QA step for non-master branches'
        }
    }
    if (currentBuild.currentResult == 'SUCCESS') {
        stage('Deploy') {
            sh 'pwd'
        }
    }
}
