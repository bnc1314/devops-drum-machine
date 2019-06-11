pipeline {
  agent {
    node { label 'ubuntu'}
  }
  stages {
    stage('Compile') {
      steps {
        sh 'npm install'
        sh 'npm run build'
      }
    }
    stage('Unit Testing') {
      steps {
        sh 'npm test'
      }
    }
    stage('Deploy') {
      parallel {
        stage('Deploy') {
          steps {
            sh 'docker stop student14'
            sh 'docker rm student14'
            sh 'docker run -d --name student14 -p 7676:80 -p 2121:22 iliyan/docker-nginx-sshd'
            sshPublisher(publishers: [sshPublisherDesc(configName: 'ngix', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/sites', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'public/**/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
          }
        }
        stage('Archive Artifacts') {
          steps {
            archiveArtifacts '**/public/**/*'
          }
        }
      }
    }
    stage('Integration testing') {
      steps {
        sleep 10
        sh 'curl localhost:7676'
      }
    }
  }
}
