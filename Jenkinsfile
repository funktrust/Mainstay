pipeline {
    environment {
    registryCredential = 'funktrust-dockerhub'
  }
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
            - env:
                volumeMounts:
                - mountPath: /var/run/docker.sock
                    name: docker-sock
            command:
            - cat
            tty: true
          volumes:
          - hostPath:
            path: /var/run/docker.sock
            type: File
          name: docker-sock
        '''
    }
  }
  stages {
    stage('Clone repository') {
      steps {
        git url:'https://github.com/funktrust/mainstay.git', branch:'master'
      }
    }

    stage('Run docker') {
      steps {
        container('docker') {
          sh 'docker build -t mainstay:latest .'
        }
      }
    }
  }
}
