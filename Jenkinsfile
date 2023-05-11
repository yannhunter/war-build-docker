node{
        def registryProjet='registry.gitlab.com/devops2921/presentation-jenkins/wartest'
        def IMAGE="${registryProjet}:version-${env.BUILD_ID}"
        
        stage('Clone') {
            checkout scm
        }

        stage('Maven package') {
            sh 'mvn package'
        }

        def img = stage('Build') {
            docker.build("$IMAGE",  '.')
        }
        stage('Run') {
            img.withRun("--name run-$BUILD_ID -p 8081:8080") { c ->
            sh 'docker ps'
            sh 'netstat -ntaup'
            sh 'sleep 30s'
            sh 'curl localhost:8081'
            sh 'docker ps'
          }
        }
        stage('Push') {
          docker.withRegistry('https://registry.gitlab.com', 'reg1') {
              img.push 'latest'
              img.push()
          }
        }


}