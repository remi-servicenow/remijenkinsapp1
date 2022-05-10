pipeline {
  agent any
  environment {
    APPSYSID = 'c6a6afba87d341102cb687780cbb358d'
    BRANCH = "${BRANCH_NAME}"
    CREDENTIALSDEV = 'e05ea163-803b-4d19-a71b-1192b74f634b'
    CREDENTIALSTEST = 'c0deef2f-2b06-45b3-8ed1-f879fadcd17a'
    CREDENTIALSPROD = 'f42e3bb7-214b-4eb7-9e09-065758ce5be6'
    DEVENV = 'https://emprstep1dev.service-now.com/'
    TESTENV = 'https://emprstep1test.service-now.com/'
    PRODENV = 'https://emprstep1prod.service-now.com/'
    TESTSUITEID = 'ab72ec6b8f033300a8616c7827bdee01'
  }
  stages {
    stage('Build') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snApplyChanges(appSysId: "${APPSYSID}", branchName: "${BRANCH}", url: "${DEVENV}", credentialsId: "${CREDENTIALSDEV}")
        snPublishApp(credentialsId: "${CREDENTIALSDEV}", appSysId: "${APPSYSID}", obtainVersionAutomatically: true, url: "${DEVENV}")
      }
    }
    stage('Test') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALSTEST}", url: "${TESTENV}", appSysId: "${APPSYSID}")
        snRunTestSuite(credentialsId: "${CREDENTIALSTEST}", url: "${TESTENV}", testSuiteSysId: "${TESTSUITEID}", withResults: true)
      }
    }
    stage('Deploy to Prod') {
      when {
        branch 'master'
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALSPROD}", url: "${PRODENV}", appSysId: "${APPSYSID}")
      }
    }
  }
}
