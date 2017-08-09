node {
    stage('Preparation') { 
      git credentialsId: 'ae36df52-130d-48e1-bbea-53a830c8185f', url: 'git@github.com:jonasvinther/ca-project.git'        
   }
    state('Test') {
        sh 'python tests.py'
    }
}