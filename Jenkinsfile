def imagename = "poulomideblala/basic-banking-system"
def dockerImage = ''
node {
 def sonarScanner = tool name: 'sonarqube', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    stage('Git checkout') {
        git(url: 'https://github.com/PoulomiDeblala/basic-banking-system.git', branch: 'main')
    }
      stage('Code Analysis'){
        withSonarQubeEnv(credentialsId: '86335c06-0eaf-4bcf-9baf-af7e14a81e15') {
            sh "${sonarScanner}/bin/sonar-scanner -Dsonar.projectKey=SonarQube -Dsonar.sources=."
        }
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
            sh "docker login -u ${env.poulomideblala} -p ${env.dockerpwd}"
            sh 'docker push poulomideblala/basic-banking-system:latest'
       }
    }
    node('kubernetes') {
        stage('Run App') {
            sh """
                kubectl get pods
            """
        }
    }
}
