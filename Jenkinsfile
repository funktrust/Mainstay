podTemplate(
    cloud: 'kubernetes', 
    label: 'workshop',
    containers: [
        containerTemplate(
            name: 'maven',
            image: 'maven:alpine',
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
    node('workshop') {
        stage('Test') {
            git url: 'https://github.com/funktrust/mainstay.git'
            container('maven') {
                sh 'docker --version'
            }
        }
    }
}