pipeline {
  agent any
  
  parameters {
  	choice choices: ['qa', 'prod'], description: 'Select enviroment for deployment', name: 'DEPLOY_TO' 
        string(name: 'BRANCH_NAME', defaultValue: 'qa', description: 'input branch name to deploy')
  }

  stages {
    stage('Copy artifact') {
      steps {
        copyArtifacts filter: 'sample', fingerprintArtifacts: true,
          projectName: 'IntegrationMultibranchPipelineExample/${BRANCH_NAME}', selector: lastCompleted()
      }
    }
    stage('Deliver') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: "vagrant-private-key", keyFileVariable: 'keyfile')]) {
          sh 'ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook --private-key=${keyfile} -i ${DEPLOY_TO}.ini playbook.yml' //Using ansible, OK
        }
      }
    }
  }
}
