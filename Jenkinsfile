pipeline {
  agent any
  environment {
    staging_server="103.117.57.55/" // app server ip 
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
    stage('Project Build'){
        steps{
            //sh 'ssh ${remote_user}@${staging_server} "rm -rf ${remote_dir}/.editorconfig .env .env.example .idea"'
            sh 'ssh ${remote_user}@${staging_server} "cd ${remote_dir} && cp .env.example .env"'
            sh 'ssh ${remote_user}@${staging_server} "cd ${remote_dir} && docker compose up --build -d"'
            sh 'ssh ${remote_user}@${staging_server} "cd ${remote_dir} && docker compose run --rm app composer install"'
        }
    }
    stage('config:cache'){
        steps{
            sh 'ssh ${remote_user}@${staging_server} "cd ${remote_dir} && docker compose run --rm app php artisan config:cache"'
        }
    }
    stage('Migration') {
      steps {
        sh 'ssh ${remote_user}@${staging_server} "cd ${remote_dir} && docker compose run --rm app php artisan migrate"'
      }
    }
  }
}
