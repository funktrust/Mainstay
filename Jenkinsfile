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
        stage('Test') {
            git url: 'https://github.com/funktrust/mainstay.git'
            container('docker') {
                sh 'docker build -t mainstay:latest .'
            }
        }
    }
}