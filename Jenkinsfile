pipeline {
  agent any
  options {
    ansiColor('xterm')
    timestamps()
    timeout(time: 30, unit: 'MINUTES')
    buildDiscarder(logRotator(numToKeepStr: '10'))
    disableConcurrentBuilds()
  }
  tools {
    jdk 'Oracle JDK 1.8'
    gradle 'Gradle 4.1'
  }
  stages {
    stage('Build') {
      steps{
 		sh './gradlew clean assembleDebug'
		archiveArtifacts artifacts: '**/build/outputs/apk/**/*debug.apk', fingerprint: true
		  }
    }
    stage('Code Quality') {
      failFast true
      parallel {
      stage('Unit Test') {
      	steps {
            sh './gradlew testDebugUnitTest'
            }
        post {
            always {
                junit '**/build/test-results/**/*.xml'
                }
            }
        }
  //  stage('Checkstyle Checks') {
  //      steps {
  //          sh './gradlew checkstyle'
  //          checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '**/reports/checkstyle/checkstyle.xml', unHealthy: ''
  //          }
  //      }
      stage('Lint Checks') {
        steps {
            sh './gradlew lintDebug'
            androidLint canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '**/build/reports/lint-results*.xml', unHealthy: ''
            }
        }
      stage('Dependency Check') {
        steps {
            sh './gradlew dependencyCheckAnalyze'
            }
          post {
            always {
              publishHTML target: [
                allowMissing: true,
                alwaysLinkToLastBuild: false,
                keepAll: true,
                reportDir: 'build/reports/',
                reportFiles: 'dependency-check-report.html',
                reportName: 'Dependency Check'
                ]
            }
          }
        }
      }
    }
  stage('Deploy to Device') {
          steps{
        sh './gradlew uninstallDevDebug installDevDebug'
               }
            }
      }
}
