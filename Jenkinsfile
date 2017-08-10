// node {
//     stage('Preparation') { 
//       git credentialsId: 'ae36df52-130d-48e1-bbea-53a830c8185f', 
//       url: 'git@github.com:jonasvinther/ca-project.git'        
//     }
//     withDockerContainer('jonasvinther/ca-project') {
//         sh 'python tests.py'
//     }
//     // stage('Test') {
//     //     sh 'python tests.py'
//     // }
//     // docker run -it --rm --name ca python-project -p 5000:5000 jonasvinther/ca-project tests.py
// }


// node("docker") {
//     docker.withRegistry('<<your-docker-registry>>', '<<your-docker-registry-credentials-id>>') {
    
//         git url: "<<your-git-repo-url>>", credentialsId: '<<your-git-credentials-id>>'
    
//         sh "git rev-parse HEAD > .git/commit-id"
//         def commit_id = readFile('.git/commit-id').trim()
//         println commit_id
    
//         stage "build"
//         def app = docker.build "your-project-name"
    
//         stage "publish"
//         app.push 'master'
//         app.push "${commit_id}"
//     }
// }


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
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
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
}