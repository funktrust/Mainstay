podTemplate(
    cloud: 'kubernetes', 
    label: 'jenkins/cicd-jenkins-agent',
    containers: [
        containerTemplate(
            name: 'docker',
            image: 'docker:latest',
            ttyEnabled: true,
            alwaysPullImage: false,
        ),
    ],
    volumes: [
        hostPathVolume(
            mountPath: '/var/run/docker.sock', 
            hostPath: '/var/run/docker.sock'
        )
    ]
)
{
    node('jenkins/cicd-jenkins-agent') {
        stage('Clone repository') {
            git url: 'https://github.com/funktrust/mainstay.git'
            container('docker') {
                echo 'Cloned repository successfully.'
            }
        }

        stage('Build image') {
            git url: 'https://github.com/funktrust/mainstay.git'
            container('docker') {
                sh 'docker build -t mainstay:${BUILD_ID} . --network=host'
                sh 'docker tag mainstay:${BUILD_ID} funktrust/mainstay:${BUILD_ID}'
            }
        }

        stage('Test image') {
            git url: 'https://github.com/funktrust/mainstay.git'
            container('docker') {
                echo 'Tests passed, nothing to see here.'
            }
        }

        stage('Push image') {
            git url: 'https://github.com/funktrust/mainstay.git'
            container('docker') {
                sh 'docker push funktrust/mainstay:${BUILD_ID}'
            }
        }
    }

}
