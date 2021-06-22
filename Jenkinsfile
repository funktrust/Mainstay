podTemplate(
    cloud: 'kubernetes', 
    label: 'jenkins-slave',
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
    node('jenkins-slave') {
        stage('Test') {
            git url: 'https://github.com/funktrust/mainstay.git'
            container('docker') {
                sh 'docker --version'
            }
        }
    }
}