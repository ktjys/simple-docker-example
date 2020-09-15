pipeline {
  agent {
    kubernetes {
      label 'kaniko-test'
      yaml '''
kind: Pod
metadata:
  name: kaniko
spec:
  containers:
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    imagePullPolicy: Always
    command:
    - /busybox/cat
    tty: true
    volumeMounts:
    - name: docker-config
      mountPath: /kaniko/.docker/
    # when not using instance role
    - name: aws-secret
      mountPath: /root/.aws/
  volumes:
  - name: docker-config
    configMap:
      name: docker-config
  # when not using instance role
  - name: aws-secret
    secret:
      secretName: aws-secret
'''
    }

  }
  stages {
    stage('build image') {
      steps {
        container(name: 'kaniko') {
          sh '''echo "PATH=$PATH"
/kaniko/executor --context `pwd` --destination  400603430485.dkr.ecr.ap-northeast-2.amazonaws.com/kaniko-test:latest'''
        }

      }
    }

  }
}