pipeline {
  agent any
  stages {
    stage('Clone cert-manager') {
      steps {
        git(url: scm.userRemoteConfigs[0].url, branch: '$BRANCH_NAME', changelog: true, credentialsId: 'KK-github-key', poll: true)
      }
    }

    stage('Clone dns-vender') {
      steps {
        dir('dns-vender') {
          git(url: scm.userRemoteConfigs[1].url, branch: '$DNS_VENDOR_BRANCH', changelog: true, credentialsId: 'KK-github-key', poll: true)
        }
      }
    }

    stage('Deploy cert-manager for dev') {
      when {
        allOf {
          expression { DEPLOY_ENV == 'dev' }
          expression { DEPLOY_TARGET == 'true' }
        }
      }
      steps {
        sh 'kubectl apply -f cert-manager.yaml'
        sh 'kubectl apply -k k8s/selfsign'
      }
    }

    stage('Deploy cert-manager for prod') {
      when {
        allOf {
          expression { DEPLOY_ENV == 'prod' }
          expression { DEPLOY_TARGET == 'true' }
        }
      }
      steps {
        script {
          if (DNS_VENDOR == 'ali') {
            sh 'kubectl apply -f cert-manager.yaml'
            sh 'kubectl apply -k k8s/alidns'
          }
          if (DNS_VENDOR == 'godaddy') {
            sh 'kubectl apply -f cert-manager.yaml'
            sh 'kubectl apply -k k8s/godaddydns'
          }
        }
      }
    }
  }

  post('Report') {
    fixed {
      script {
        sh(script: 'bash $JENKINS_HOME/wechat-templates/send_wxmsg.sh fixed')
     }
      script {
        // env.ForEmailPlugin = env.WORKSPACE
        emailext attachmentsPattern: 'TestResults\\*.trx',
        body: '${FILE,path="$JENKINS_HOME/email-templates/success_email_tmp.html"}',
        mimeType: 'text/html',
        subject: currentBuild.currentResult + " : " + env.JOB_NAME,
        to: '$DEFAULT_RECIPIENTS'
      }
     }
    success {
      script {
        sh(script: 'bash $JENKINS_HOME/wechat-templates/send_wxmsg.sh successful')
     }
      script {
        // env.ForEmailPlugin = env.WORKSPACE
        emailext attachmentsPattern: 'TestResults\\*.trx',
        body: '${FILE,path="$JENKINS_HOME/email-templates/success_email_tmp.html"}',
        mimeType: 'text/html',
        subject: currentBuild.currentResult + " : " + env.JOB_NAME,
        to: '$DEFAULT_RECIPIENTS'
      }
     }
    failure {
      script {
        sh(script: 'bash $JENKINS_HOME/wechat-templates/send_wxmsg.sh failure')
     }
      script {
        // env.ForEmailPlugin = env.WORKSPACE
        emailext attachmentsPattern: 'TestResults\\*.trx',
        body: '${FILE,path="$JENKINS_HOME/email-templates/fail_email_tmp.html"}',
        mimeType: 'text/html',
        subject: currentBuild.currentResult + " : " + env.JOB_NAME,
        to: '$DEFAULT_RECIPIENTS'
      }
     }
    aborted {
      script {
        sh(script: 'bash $JENKINS_HOME/wechat-templates/send_wxmsg.sh aborted')
     }
      script {
        // env.ForEmailPlugin = env.WORKSPACE
        emailext attachmentsPattern: 'TestResults\\*.trx',
        body: '${FILE,path="$JENKINS_HOME/email-templates/fail_email_tmp.html"}',
        mimeType: 'text/html',
        subject: currentBuild.currentResult + " : " + env.JOB_NAME,
        to: '$DEFAULT_RECIPIENTS'
      }
     }
  }
}
