#!/usr/bin/env groovy
 
/**
        * Jenkinsfile for Jenkins2 Pipeline
*/
 
import hudson.model.*
import hudson.EnvVars
import groovy.json.JsonSlurperClassic
import groovy.json.JsonBuilder
import groovy.json.JsonOutput
import java.net.URL

node {
  // Mark the code checkout 'stage'....
  stage ('Stage Checkout') {

  // Checkout code from repository and update any submodules
  checkout scm
  sh 'git submodule update --init'  
  }
  
  stage ('Stage Build') {

  //branch name from Jenkins environment variables
  echo "My branch is: ${BRANCH_NAME}"
  
  echo "My SDK version is: ${env.PATH} and ${env.ANDROID_HOME}"

  //build your gradle flavor, passes the current build number as a parameter to gradle
  sh 'chmod +x gradlew; ./gradlew clean assemble'
  }

  stage ('Stage Archive') {
  //tell Jenkins to archive the apks
  archiveArtifacts artifacts: 'app/build/outputs/apk/*.apk', fingerprint: true
  }

  stage ('Stage Upload To Fabric') {
  sh './gradlew crashlyticsUploadDistribution${flavor}Debug  -PBUILD_NUMBER=${BUILD_NUMBER}'
  }
}