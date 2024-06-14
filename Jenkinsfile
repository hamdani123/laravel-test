pipeline {
  agent any
  environment {
    staging_server="103.117.57.55" // app server ip 
    remote_dir="/home/aplikasi/test/" // app server directory
    remote_user="aplikasi" // app server user
  }
  stages {
    stage('Deploy') {
      steps {
        sh 'rsync -avP --exclude ".env" --exclude "vendor" --exclude ".git" --exclude "docker" --exclude="storage" --delete ${WORKSPACE}/ ${remote_user}@${staging_server}:${remote_dir}'
        sh 'scp -r ${WORKSPACE}/docker ${remote_user}@${staging_server}:${remote_dir}'
      }
    }
  }
}
