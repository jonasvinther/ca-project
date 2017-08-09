node {
    stage('Preparation') { 
      git credentialsId: 'ae36df52-130d-48e1-bbea-53a830c8185f', 
      url: 'git@github.com:jonasvinther/ca-project.git'        
    }
    withDockerContainer('jonasvinther/ca-project') {
        sh 'python tests.py'
    }
    // stage('Test') {
    //     sh 'python tests.py'
    // }
    // docker run -it --rm --name ca python-project -p 5000:5000 jonasvinther/ca-project tests.py
}


