pipeline {
  agent any
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/cjone112/cjone-pub-repo/tree/echo-ip-1', branch: 'echo-ip-1'
      }
    }
    stage('docker build and push') {
      steps {
        sh '''
        docker login -u S1sauVbDBA4NrJZhunNG -p QFzSO0phOYyOG8YQ 61840435-kr1-registry.container.nhncloud.com/cjone-test-reg
        docker build -t 61840435-kr1-registry.container.nhncloud.com/cjone-test-reg/echo-ip .
        docker push 61840435-kr1-registry.container.nhncloud.com/cjone-test-reg/echo-ip
        '''
      }
    }
    stage('deploy kubernetes') {
      steps {
        sh '''
        export KUBECONFIG=/var/lib/jenkins/cjone-kube-test_kubeconfig.yaml
        kubectl create deployment fs-echo-ip --image=61840435-kr1-registry.container.nhncloud.com/cjone-test-reg/echo-ip
        kubectl expose deployment fs-echo-ip --type=LoadBalancer --name=pl-echo-ip-svc --port=8080 --target-port=80
        '''
      }
    }
  }
}
