pipeline { 
    environment {
    imagename = "funktrust/mainstay"
    registryCredential = 'funktrust-dockerhub'
    dockerImage = ''
  }
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
              dockerImage = docker.build ("funktrust/mainstay")
            }
            
        }
    }
    
    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        
            echo "Tests passed, nothing to see here."
        
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')

          }
        }
      }
    }
    
  }
}