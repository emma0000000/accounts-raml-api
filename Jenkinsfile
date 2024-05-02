def config = {}
def env = {}
pipeline {
  agent any
  triggers{
    pollSCM('H/2 * * * *')
  }
  stages {
    stage("Configuration setup..."){
            steps{
                echo "Configuration setup..."
                script{
                    config = readJSON file:"env/${env.BRANCH_NAME}/config.json"
                    env = config.get("envConfig")
                }
            }
    }
stage('Build Application') {
      steps {
        sh 'mvn clean install'
      }  
    }
    stage('Deploy CloudHub') {
      environment {
        ANYPOINT_CREDENTIALS = credentials('1f1a2545-1ea7-4fb0-a65b-19ea1d9a3f88')
      }
      steps {
        sh "mvn clean deploy -DmuleDeploy -Dcloud.env=${env.envName} -DcloudhubAppName=${env.cloudhubAppName} -Dmule.version=${env.muleVersion} -Dcloud.user=${ANYPOINT_CREDENTIALS_USR} -Dcloud.password=${ANYPOINT_CREDENTIALS_PSW} -DworkerType=Micro -Dworkers=1"
      }
    }
  }
}

   



