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

// *** Initialization ***

gitHash = ""

// *** Pipeline steps ***

node {
    stage('Build-Scan') {
      parallel(
         'Build artifact': {
            echo "Build step"
            sh 'echo "The build file" > build.txt'
         },
         'Scan source code': {
            echo "Scan step"
            sh 'echo "The scan report shows success" > scan.txt'
         }
      )
    }
    stage('Create Artifact') {
      artifactName = "artifacts/${env.BUILD_ID}-${env.Branch_Name}.zip"
      sh 'zip ${artifactName} build.txt'
    }
    stage('OK to Proceed to QA') {
      input message: 'Proceed with QA deployment?', ok: 'Yes'
    }
    stage('QA') {
        if (env.BRANCH_NAME == 'master') {
            echo 'Deploy to QA and execute Full QA testing'
        } else {
            echo 'Deploy to QA and Quick smoke test'
        }
    }
    stage('All clear from QA') {
      input message: 'QA validation successfully completed?', ok: 'Yes'
    }
    stage('Initiate Prod Deployment') {
       build job: "Operations/Prod-Deploy", propagate: true, wait: true, parameters:
          [string(name: 'artifact', value: "${artifactName}"),
          string(name: 'branch', value: "${env.Branch_Name}"),
          string(name: 'pipelineBuildNumber', value: "${env.BUILD_ID}")]
    }
//    if (currentBuild.currentResult == 'SUCCESS') {
//        stage('Deploy') {
//            sh 'pwd'
//        }
//    }
}
