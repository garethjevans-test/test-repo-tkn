def podTemplate = """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: tkn
    image: garethjevans/tkn:0.15.0
    command:
    - cat
    tty: true
"""

pipeline {
  agent {
    kubernetes {
      yaml podTemplate
    }
  }

  options {
    buildDiscarder(logRotator(numToKeepStr: '10'))
    timeout(time: 30, unit: 'MINUTES')
    disableConcurrentBuilds()
  }

  stages {
    stage('Validate') {
      steps {
        container('tkn') {
          sh 'tkn version'
        }
      }
    }
    //stage('Plan') {
    //  when { changeRequest() }
    //  steps {
    //    container('terraform') {
    //      script {
    //        plan = sh(returnStdout: true, script: 'terraform plan -no-color')
    //        pullRequest.comment('```\n' + plan)
    //      }
    //    }
    //    // jnlp has a shell available
    //    sh 'curl --silent --location --show-error --output tfsec https://github.com/tfsec/tfsec/releases/download/v0.36.9/tfsec-linux-amd64'
    //    sh 'chmod a+x tfsec'
    //    sh './tfsec'
    //  }
    //}
  }
}