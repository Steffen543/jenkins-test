node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
		bat 'SET DOCKER_TLS_VERIFY=1'
		bat 'SET DOCKER_HOST=tcp://192.168.99.100:2376'
		bat 'SET DOCKER_CERT_PATH=C:\Users\Steff\.docker\machine\machines\default'
		bat 'SET DOCKER_MACHINE_NAME=default'
        app = docker.build("steffenkoch/hellonodehome")
    }

   

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
