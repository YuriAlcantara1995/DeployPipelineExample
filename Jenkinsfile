pipeline {
  agent any

  stages {
    stage('Copy artifact') {
      steps {
        copyArtifacts filter: 'sample', fingerprintArtifacts: true,
          projectName: 'sample-multibranch/mathias', selector: lastSuccessful()
      }
    }
    stage('Deliver') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: "vagrant-private-key", keyFileVariable: 'keyfile')]) {
          sh 'ansible-playbook --private-key=${keyfile} -i hosts.ini playbook.yml'
            // sh 'scp -o "StrictHostKeyChecking=no" -i ${keyfile} ./sample vagrant@10.10.50.3:'
        }
      }
    }

  }
}
