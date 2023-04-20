def config = {}
def env = {}

pipeline {
  agent any
  environment {
		DEPLOY_CREDS = credentials('deploy-anypoint-user')
  } 
  stages {
   stage('config') {
      steps {
         script {
            config = readJSON file: 'env/${env.BRANCH_NAME}/config.json'
         }
      }
    }
   stage('Build') {
      steps {
          sh "mvn clean package"
      }
    }

    stage('Deploiment CloudHub') { 
      steps {
          sh "mvn -e -X deploy -DmuleDeploy -Dmule.version=${config.env.MULE_VERSION} -Danypoint.username=${DEPLOY_CREDS_USR} -Danypoint.password=${DEPLOY_CREDS_PSW} -Dcloudhub.app=${config.env.APP_NAME}-${config.env.ENVIRONMENT.toLowerCase()} -Dcloudhub.environment=${config.env.ENVIRONMENT} -Dcloudhub.bg=${config.env.BG} -Dcloudhub.bgid=${config.env.BGID}  -Dcloudhub.worker=${config.env.WORKERS} -Dcloudhub.workersize=${config.env.WORKERSIZE} -Dcloudhub.region=${config.env.REGION}"
      }
    }
   }
}

							