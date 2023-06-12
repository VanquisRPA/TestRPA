pipeline {
   agent any

 


       // Environment Variables
       environment {
       MAJOR = '1'
       MINOR = '0'
       //Orchestrator Services
       UIPATH_ORCH_URL = "https://cloud.uipath.com/synecgnjrtxq/DefaultTenant/orchestrator_/"
       UIPATH_ORCH_LOGICAL_NAME = "synechzkmwol"
       UIPATH_ORCH_TENANT_NAME = "VanquisBankPOC"
       UIPATH_ORCH_FOLDER_NAME = "Shared"
   }

 


   stages {

 


       // Printing Basic Information
       stage('Preparing'){
           steps {
        cleanWs()
               echo "Jenkins Home ${env.JENKINS_HOME}"
               echo "Jenkins URL ${env.JENKINS_URL}"
               echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
               echo "Jenkins JOB Name ${env.JOB_NAME}"
               echo "GitHub BranchName ${env.BRANCH_NAME}"
               checkout scm

 


           }
       }

 


        // Building Tests
       stage('Build Tests') {
           steps {
               echo "Building package with ${WORKSPACE}"
               UiPathPack (
              outputPath: "Output\\${env.BUILD_NUMBER}",
              outputType: 'Tests',
                     projectJsonPath: "project.json",
                     version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
                     useOrchestrator: false,
                     traceLevel: 'Error'
)
           }
       }

 

// Deploy Stages
       stage('Deploy Tests') {
           steps {
               echo "Deploying ${BRANCH_NAME} to orchestrator"
               UiPathDeploy (
               createProcess:true,
               packagePath: "Output\\${env.BUILD_NUMBER}",
               orchestratorAddress: "${UIPATH_ORCH_URL}",
               orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
               folderName: "${UIPATH_ORCH_FOLDER_NAME}",
               environments: "${UIPATH_ORCH_FOLDER_NAME}",
               //credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'APIUserKey']
               credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'APIUserKey'),
                traceLevel: 'Error',
               entryPointPaths: 'Main.xaml'

 


)
           }

 

}

 

   }           
}
