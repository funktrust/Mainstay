pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        metadata:
          labels:
            some-label: some-label-value
        spec:
          containers:
          - name: docker
            image: docker:latest
            command:
            - cat
            tty: true
          - name: busybox
            image: busybox
            command:
            - cat
            tty: true
        '''
    }
  }
  stages {
    stage('Run docker') {
      steps {
        container('docker') {
          sh 'docker --version'
        }
        container('busybox') {
          sh '/bin/busybox'
        }
      }
    }
  }
}
