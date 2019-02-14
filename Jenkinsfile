
def awsCreds = "ecr_push_pull"
def awsRegion = "eu-west-1"
def ecrRepo =  "024942195839.dkr.ecr.eu-west-1.amazonaws.com/stubrownuk123"
def ecrTag = "latest"
def awscliContainer = "mesosphere/aws-cli:1.14.5"
def dockerContainer = "docker:18.06"


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
    image: ${awscliContainer}
    command:
    - cat
    tty: true
  - name: docker
    image: ${dockerContainer}
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
                  withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: awsCreds, secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                      script{
                          sh "aws ecr get-login --region ${awsRegion} --no-include-email > lgn"
                          env.dockerLogin = readFile 'lgn'
                      }
                  }
              }

              container('docker'){
                  sh "${env.dockerLogin}"
                  sh "docker build . -t ${ecrRepo}:${ecrTag}"
                  sh "docker push ${ecrRepo}:${ecrTag}"

              }
          }
      }
  }
}




