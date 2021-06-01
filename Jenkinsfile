pipeline {
  agent any
  stages {
    stage('Preparing') {
      steps {
        echo "Jenkins Home ${env.JENKINS_HOME}"
        echo "Jenkins URL ${env.JENKINS_URL}"
        echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
        echo "Jenkins JOB Name ${env.JOB_NAME}"
        echo "GitHub BranhName ${env.BRANCH_NAME}"
        checkout scm
      }
    }

    stage('Build') {
      steps {
        echo "Building..with ${WORKSPACE}"
        UiPathPack(outputPath: "Output\\${env.BUILD_NUMBER}", projectJsonPath: 'project.json', version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"])
      }
    }

    stage('Test') {
      steps {
        echo 'Testing..the workflow...'
      }
    }

    stage('Deploy to UAT') {
      steps {
        echo "Deploying ${BRANCH_NAME} to TEST "
        UiPathDeploy(packagePath: "Output\\${env.BUILD_NUMBER}", orchestratorAddress: "${UIPATH_ORCH_URL}", orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}", folderName: "${UIPATH_ORCH_FOLDER_NAME}", environments: 'DEV', credentials: UserPass(credentialsId: 'nx5QjC8ha6HQxYawEeSEhLRyikoBZJjvq0VzXe4XpPz3n'))
      }
    }

    stage('Deploy to Production') {
      steps {
        echo 'Deploy to Production'
      }
    }

  }
  environment {
    MAJOR = '1'
    MINOR = '0'
    UIPATH_ORCH_URL = 'https://cloud.uipath.com/'
    UIPATH_ORCH_LOGICAL_NAME = 'sunhome'
    UIPATH_ORCH_TENANT_NAME = 'sunhomeDefault'
    UIPATH_ORCH_FOLDER_NAME = 'Default'
  }
  post {
    success {
      echo 'Deployment has been completed!'
    }

    failure {
      echo "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JOB_DISPLAY_URL})"
    }

    always {
      cleanWs()
    }

  }
  options {
    timeout(time: 80, unit: 'MINUTES')
    skipDefaultCheckout()
  }
}