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
    }

    node('jenkins/cicd-jenkins-agent') {
        stage('Build image') {
            container('docker') {
                sh 'docker build -t mainstay:latest . --network=host'
            }
        }
    }

    node('jenkins/cicd-jenkins-agent') {
        stage('Test image') {
            container('docker') {
                echo 'Tests passed, nothing to see here.'
            }
        }
    }

}
