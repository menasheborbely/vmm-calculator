pipeline {

  agent any
  environment {
    //adding a comment for the commit test
    DEPLOY_CREDS = credentials('vmm-deploy-user')
    MULE_VERSION = '4.3.0'
    BG = "BH IME"
    WORKER = "Micro"
  }
  stages {
    stage('Build') {
      steps {
      		sh 'mvn clean'
            sh 'mvn -B -U -e -V -DskipTests package'
      }
    }

    stage('Test') {
      steps {
          sh "mvn test"
      }
    }

     stage('Deploy Development') {
      environment {
        ENVIRONMENT = 'Sandbox'
        APP_NAME = 'vmm-calculator'
      }
      when {
    	expression {
        	return env.GIT_BRANCH == 'origin/develop';
        }
      }
      steps {
            sh "mvn -U -V -e -B -DskipTests deploy -DmuleDeploy -Dmule.version=\"$MULE_VERSION\" -Danypoint.username=\"$DEPLOY_CREDS_USR\" -Danypoint.password=\"$DEPLOY_CREDS_PSW\" -Dcloudhub.app=\"$APP_NAME\" -Dcloudhub.environment=\"$ENVIRONMENT\" -Dcloudhub.bg=\"$BG\" -Dcloudhub.worker=\"$WORKER\""
      }
    }
    stage('Deploy Production') {
      environment {
        ENVIRONMENT = 'Sandbox'
        APP_NAME = 'vmm-calculator-prod'
      }
      when {
    	expression {
        	return env.GIT_BRANCH == 'origin/master';
        }
      }
      steps {
            sh "mvn -U -V -e -B -DskipTests deploy -DmuleDeploy -Dmule.version=\"$MULE_VERSION\" -Danypoint.username=\"$DEPLOY_CREDS_USR\" -Danypoint.password=\"$DEPLOY_CREDS_PSW\" -Dcloudhub.app=\"$APP_NAME\" -Dcloudhub.environment=\"$ENVIRONMENT\" -Dcloudhub.bg=\"$BG\" -Dcloudhub.worker=\"$WORKER\""
      }
    }
  }

  tools {
    maven 'M3'
  }
}