pipeline {
  agent any
  
  parameter {
  	choice choices: ['qa', 'prod'], description: 'Select enviroment for deployment', name: 'DEPLOY_TO' 
  }

  stages {
    stage('Copy artifact') {
      steps {
        copyArtifacts filter: 'sample', fingerprintArtifacts: true,
          projectName: 'IntegrationMultibranchPipelineExample/qa', selector: lastSuccessful()
      }
    }
    stage('Deliver') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: "vagrant-private-key", keyFileVariable: 'keyfile')]) {
          sh 'ansible-playbook --private-key=${keyfile} -i ${DEPLOY_TO}.ini playbook.yml' //Using ansible, OK
          //sh 'scp -o "StrictHostKeyChecking=no" -i ${keyfile} ./sample vagrant@10.10.50.3:' //Using scp, OK
        }
      }
    }

  }
}
