def podTemplate = """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: tkn
    image: garethjevans/tkn:0.15.1
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
          sh 'kubectl apply -f task.yaml -n tekton-pipelines'
          sh 'tkn task describe echo-hello-world'
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
    //  }
    //}
  }
}
