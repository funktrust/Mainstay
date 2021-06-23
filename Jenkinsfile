pipeline {
    agent {
        kubernetes {
            yaml """\
        apiVersion: v1
        kind: Pod
        metadata:
            name: 'jenkins/cicd-jenkins-agent'
            namespace: 'jenkins'
        volumes:
            hostPathVolume:
            - mountPath: '/var/run/docker.sock'
            - hostPath: '/var/run/docker.sock'
        spec:
            containers:
            - name: docker
              image: docker:latest
              imagePullSecrets:
                  - name: dockerhub
              tty: true

        """.stripIndent()
        }
    }
    stages {
        stage('Clone repository') {
                steps {
                    git url: 'https://github.com/funktrust/mainstay.git'
                    container('docker') {
                        echo 'Cloned repository successfully.'
                    }
                }
            }
    }
}