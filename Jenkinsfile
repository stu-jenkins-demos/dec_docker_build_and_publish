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
  - name: docker
    image: docker:18.06
    command: ["cat"]
    tty: true
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-socket
  volumes:
  - name: docker-socket
    hostPath:
      path: /var/run/docker.sock
      type: Socket
"""
    }
  }
  stages {
      stage('BuildImage') {
          steps {
              container('awscli'){
                  withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'ecr_push_pull', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                      script{
                          sh 'aws ecr get-login --region eu-west-1 --no-include-email > lgn'
                          env.dockerLogin = readFile 'lgn'
                      }
                  }
              }

              container('docker'){
                  sh "${env.dockerLogin}"
                  sh "docker build . -t 024942195839.dkr.ecr.eu-west-1.amazonaws.com/stubrownuk123"
                  sh "docker push 024942195839.dkr.ecr.eu-west-1.amazonaws.com/stubrownuk123"

              }
          }
      }
  }
}




