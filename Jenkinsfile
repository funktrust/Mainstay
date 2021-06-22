pipeline {  
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker:19.03
    command:
    - cat
    tty: true
    privileged: true
"""
    }
  }
  stages {
    stage('Clone repository') {
      steps {
        container('docker') {
        /* Let's make sure we have the repository cloned to our workspace */

        /*checkout scm*/
        checkout ([
            $class: 'GitSCM',
            branches: [[name: "${params.REVISION}"]],
            userRemoteConfigs: [[
            url: 'https://github.com/funktrust/mainstay.git']]
                   ])
        }
      }
    }

    stage('Build image') {
        steps {
            container('docker') {
              app = docker.build("funktrust/mainstay")
            }
            
        }
    }
    
  }
}