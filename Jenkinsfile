pipeline {
  agent any

  stages {
    stage('Copy artifact') {
      steps {
        copyArtifacts filter: 'sample', fingerprintArtifacts: true, selector: lastSuccessful()
      }
    }
    stage('Deliver') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: "1b54f700-2045-4984-9830-9848b2fe5a39", keyFileVariable: 'keyfile')]) {
          sh 'ansible-playbook --private-key=${keyfile} -i hosts.ini playbook.yml'
            // sh 'scp -o "StrictHostKeyChecking=no" -i ${keyfile} ./sample vagrant@10.10.50.3:'
        }
      }
    }

  }
}
