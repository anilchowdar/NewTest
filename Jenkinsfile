//Declarative Pipeline
pipeline {
  agent any
  options {
    skipDefaultcheckout()
  }
  stages {
    stage('Checkout Scm') {
      steps {
        git(url: 'https://github.com/anilchowdar/TestcProgram.git', credentialsId: '89678e7e-057b-4d24-a0b4-2878dbe5d38d')
      }
    }

    stage('Shell script 0') {
      steps {
          //Getting Dynamic WorkSpace Path
          sh 'echo "WP=$WORKSPACE"'
        sh '''#!/bin/bash
sudo python3 -m robot $WORKSPACE/*.robot
chmod +x $WORKSPACE/*.robot'''
/* groovylint-disable-next-line LineLength */
robot archiveDirName: 'robot-plugin', outputPath: '', overwriteXAxisLabel: '', passThreshold: 90.0, unstableThreshold: 10.0
      }
    }
}
  post {
    always {
      //Archiving Results from Current Build
      step($class: 'ArtifactArchiver', artifacts: '**/*', followSymlinks: false)
      //Publish HTML Reports
      /* groovylint-disable-next-line LineLength */
      publishHTML(alwaysLinkToLastBuild: false, allowMissing: false, reportDir: '', keepAll: false, reportTitles: '', reportName: 'HTML Report', reportFiles: 'report.html')
      //WorkSpace Cleanup
      step([$class: 'WsCleanup'])
      //Build Number 
      echo "BUILD_NUMBER = $BUILD_NUMBER"
      //Display name
      echo "BUILD_DISPLAY_NAME = $BUILD_DISPLAY_NAME"
     }
   }
  triggers {
  /* groovylint-disable-next-line LineLength */
  githubPullRequests abortRunning: true, cancelQueued: true, events: [], preStatus: true, skipFirstRun: true, spec: '', triggerMode: 'HEAVY_HOOKS'
  pollSCM ''
   }
}
