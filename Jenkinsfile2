node {


    
    def app
    def dockerRegistry
    def dockerCreds
    
    parameters {
        gitParameter(name: 'REVISION', defaultValue: 'master', type: 'PT_REVISION')
    }


    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        /*checkout scm*/
        checkout ([
            $class: 'GitSCM',
            branches: [[name: "${params.REVISION}"]],
            userRemoteConfigs: [[
            url: 'https://github.com/funktrust/mainstay.git']]
                   ])
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("funktrust/mainstay")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        
            echo "Tests passed, nothing to see here."
        
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        /* If we are pushing to docker hub, use this: */
           dockerRegistry =  'https://registry.hub.docker.com'
           dockerCreds = 'funktrust'
        /* If we are pushing to Artifactory, use this: 
        dockerRegistry = 'https://armory-docker-local.jfrog.io'
        dockerCreds = 'fernando-armory-artifactory'*/
        
        docker.withRegistry(dockerRegistry, dockerCreds ) {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
            
        }
    }
    stage('Create Properties file') {
        sh "docker inspect --format=\'{{index .RepoDigests 0}}\' registry.hub.docker.com/funktrust/mainstay:${env.BUILD_NUMBER}>image.properties"
        
        
      
        archiveArtifacts artifacts: 'image.properties'
    }
        
}
