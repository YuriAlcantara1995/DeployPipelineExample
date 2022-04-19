pipeline {
  agent any
  
  parameters {
  	choice choices: ['qa', 'prod'], description: 'Select enviroment for deployment', name: 'DEPLOY_TO' 
        string(name: 'PROJECT_NAME', defaultValue: 'IntegrationMultibranchPipelineExample/qa', description: 'input project to deploy')
  }

  stages {
    stage('Copy artifact') {
      steps {
        copyArtifacts filter: 'sample', fingerprintArtifacts: true,
          projectName: '${PROJECT_NAME}', selector: lastSuccessful()
      }
    }
    stage('Deliver to QA') {
      	when {
    		expression { 
        		params.DEPLOY_TO == 'qa' 
    		}
	}

      steps {
        withCredentials([sshUserPrivateKey(credentialsId: "vagrant-private-key", keyFileVariable: 'keyfile')]) {
          sh 'ansible-playbook --private-key=${keyfile} -i ${DEPLOY_TO}.ini playbook.yml' //Using ansible, OK
          //sh 'scp -o "StrictHostKeyChecking=no" -i ${keyfile} ./sample vagrant@10.10.50.3:' //Using scp, OK
        }
      }
    }
    stage('Deliver to PROD') {
      	when {
    		expression { 
        		params.DEPLOY_TO == 'prod' 
    		}
	}

      steps {
        withCredentials([sshUserPrivateKey(credentialsId: "vagrant-private-key", keyFileVariable: 'keyfile')]) {
          sh 'ansible-playbook --private-key=${keyfile} -i ${DEPLOY_TO}.ini playbook.yml' //Using ansible, OK
          //sh 'scp -o "StrictHostKeyChecking=no" -i ${keyfile} ./sample vagrant@10.10.50.4:' //Using scp, OK
        }
      }
    }
  }
}
