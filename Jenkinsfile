def imagename = "poulomideblala/basic-banking-system"
def dockerImage = ''

 

node {
    stage('Cloning Git') {
        git(url: 'https://github.com/PoulomiDeblala/basic-banking-system.git', branch: 'main')
    }
    stage('Build Project') {
        sh "npm i"
        sh "npm run"
    }
    stage('Building image') {
        script {
          dockerImage = docker.build imagename
      }
    }
    stage('Deploy Image') {
         withCredentials([usernamePassword(credentialsId: '00aa31c4-f238-4584-abba-5f79a08b2201', passwordVariable: 'dockerpwd', usernameVariable: 'poulomideblala')]) 
    // some block
       {
            sh "docker login -u ${env.DocCredUser} -p ${env.DocCredPassword}"
            sh 'docker push poulomideblala/basic-banking-system:latest'
       }
    }
    node('Kubes') {
        stage('Run App') {
            sh """
                kubectl get pods
            """
        }
    }
}
