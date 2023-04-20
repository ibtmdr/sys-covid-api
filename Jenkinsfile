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
            config = readJSON file: "env/${env.BRANCH_NAME}/config.json"
            env = config.get("env")
         }
      }
    }
   stage('Build') {
      steps {
          sh "mvn clean install"
      }
    }

    stage('Deploiment CloudHub') { 
    
      steps {
          sh "mvn -e -X deploy -DmuleDeploy -Dmule.version=${env.MULE_VERSION} -Danypoint.username=${DEPLOY_CREDS_USR} -Danypoint.password=${DEPLOY_CREDS_PSW} -Dcloudhub.app=${env.APP_NAME}-${env.ENVIRONMENT.toLowerCase()} -Dcloudhub.environment=${env.ENVIRONMENT} -Dcloudhub.bg=${env.BG} -Dcloudhub.bgid=${env.BGID}  -Dcloudhub.worker=${env.WORKERS} -Dcloudhub.workersize=${env.WORKERSIZE} -Dcloudhub.region=${env.REGION}"
      }
    }
   }
}

							