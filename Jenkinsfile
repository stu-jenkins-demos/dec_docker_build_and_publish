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
              container('awscli'){
                  withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'ecr_push_pull', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {

                      script{
                          sh 'aws ecr get-login --region eu-west-1 > lgn'
                          //sh "echo foo > result";
                          sh 'echo check'
                          def tfile = new File('lgn').readLines()
                          return tfile[1..tfile.size-1].join("")
                          //def output=readFile('lgn').trim()
                          echo "output=$tfile";
                      }
                  }

              }

              container('busybox'){
                echo 'building an vimage'
        }
      }
    }
  }
    // 024942195839.dkr.ecr.eu-west-1.amazonaws.com/stubrownuk
    // ecr_push_pull
}




