node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("jonasvinther/ca-project")
    }

    stage('Test image') {
        /* Run tests against our image. */

        app.inside {
            sh 'python tests.py'
        }
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

    stage('Stop running container') {
        try {
            sh 'ssh root@207.154.223.214 docker stop ca-project'
            sh 'ssh root@207.154.223.214 docker rm ca-project'
        }
        catch(Exception e) {
            echo 'No running container found!'
        } 
    }

    stage('Deploy to production') {
        sh 'ssh root@207.154.223.214 docker pull jonasvinther/ca-project'
        sh 'ssh root@207.154.223.214 docker run -d --name ca-project --restart=always -p 80:5000 jonasvinther/ca-project'
    }

    stage('Functional test') {
        sh 'sleep 10';
        sh 'curl -I --silent http://207.154.223.214 | grep "200 OK"'
    }
}

post {
    always {
        echo 'One way or another, I have finished'
        deleteDir() /* clean up our workspace */
    }
    success {
        echo 'I succeeeded!'
    }
    unstable {
        echo 'I am unstable :/'
    }
    failure {
        echo 'I failed :('
    }
    changed {
        echo 'Things were different before...'
    }
}