pipeline{
  agent any

  stages{
    stage('Clone git repo'){
      steps{
        git url: 'git@github.com:DenisDoubinin/hosting.git',
          credentialsId: 'hosting-github-key'
      }
    }
    stage('Create VM'){
      steps{
        sh 'vagrant up'
      }
    }
    stage('Hosting deploy'){
      steps{
        ansiblePlaybook credentialsId: 'jenkins_node', disableHostKeyChecking: true, installation: 'ansible', inventory: 'ansible/inv.yaml', playbook: 'ansible/wp.yaml'
      }
    }
  }
  post {
        success {
            slackSend (channel: "#mynotification", tokenCredentialId: "mynotification-slack-token", color: '#00FF00', message: "SUCCESSFULL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        failure {
            slackSend (channel: "#mynotification", tokenCredentialId: "mynotification-slack-token", color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
    }
}
