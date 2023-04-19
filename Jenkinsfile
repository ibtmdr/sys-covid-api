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
          sh "mvn clean -DskipTests package"
      }
    }

    stage('Test') {
      steps {
          sh "mvn test"
      }
    }
    stage('Deploiment CloudHub to Sandbox') {
      when {
          branch 'dev'
      } 
      environment {
          ENVIRONMENT= "Sandbox"
      }
      steps {
          sh "mvn -e -X clean package deploy -DmuleDeploy -Dmule.version=${MULE_VERSION} -Danypoint.username=${DEPLOY_CREDS_USR} -Danypoint.password=${DEPLOY_CREDS_PSW} -Dcloudhub.app=${APP_NAME}-${ENVIRONMENT.toLowerCase()} -Dcloudhub.environment=${ENVIRONMENT} -Dcloudhub.bg=${BG} -Dcloudhub.bgid=${BGID}  -Dcloudhub.worker=${WORKERS} -Dcloudhub.workersize=${WORKERSIZE} -Dcloudhub.region=${REGION}"
      }
    }
    stage('Deploiment CloudHub to Design') { 
      when {
          branch 'main'
      }
      environment {
          ENVIRONMENT= "Develop"
      }
      steps {
          sh "mvn -e -X clean package deploy -DmuleDeploy -Dmule.version=${MULE_VERSION} -Danypoint.username=${DEPLOY_CREDS_USR} -Danypoint.password=${DEPLOY_CREDS_PSW} -Dcloudhub.app=${APP_NAME}-${ENVIRONMENT.toLowerCase()} -Dcloudhub.environment=${ENVIRONMENT} -Dcloudhub.bg=${BG} -Dcloudhub.bgid=${BGID}  -Dcloudhub.worker=${WORKERS} -Dcloudhub.workersize=${WORKERSIZE} -Dcloudhub.region=${REGION}"
      }
    }
  }
}

							