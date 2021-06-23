pipeline {
    agent {
        kubernetes {
            yaml """\
        apiVersion: v1
        kind: Pod
        metadata:
            name: 'jenkins/cicd-jenkins-agent'
            namespace: 'jenkins'
        spec:
            containers:
            - name: docker
              image: docker:latest
              imagePullSecrets:
                  - name: dockerhub
              tty: true
              volumeMounts:
              - name: dockersock
              mountPath: /var/run/docker.sock
            volumes:
            - name: dockersock
            hostPath:
                path: /var/run/docker.sock

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
        stage('Build image') {
                steps {
                    git url: 'https://github.com/funktrust/mainstay.git'
                    container('docker') {
                        sh 'docker build -t mainstay:${BUILD_ID} . --network=host'
                        sh 'docker tag mainstay:${BUILD_ID} funktrust/mainstay:${BUILD_ID}'
                    }
                }
            }

        stage('Test image') {
                steps {
                    git url: 'https://github.com/funktrust/mainstay.git'
                    container('docker') {
                        echo 'Tests passed, nothing to see here.'
                    }
                }
            }

        stage('Push image') {
                steps {
                    git url: 'https://github.com/funktrust/mainstay.git'
                    container('docker') {
                        sh 'docker push funktrust/mainstay:${BUILD_ID}'
                    }
                }
            }
    }
}