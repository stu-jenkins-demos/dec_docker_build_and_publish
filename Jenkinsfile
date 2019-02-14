pipeline {
  agent {
    kubernetes {
      label 'dec_docker_build_and_publish'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-label-value
spec:
  containers:
  - name: awscli
    image: mesosphere/aws-cli:1.14.5
    command:
    - cat
    tty: true
  - name: busybox
    image: busybox
    command:
    - cat
    tty: true
"""
    }
  }
  stages {
    stage('BuildImage') {
      steps {
        echo 'building an vimage'
      }
    }
  }
}