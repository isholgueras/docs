pipeline {
  agent { label 'www.tugboat.qa' }

  stages {
    stage('Deploy') {
      when {
        branch 'main'
      }

      environment {
        HUGO = 'https://github.com/gohugoio/hugo/releases/download/v0.58.3/hugo_0.58.3_Linux-64bit.tar.gz'
      }

      steps {
        echo 'Deploying....'
        sh 'curl -Ls "$HUGO" | tar -zxf - hugo'
        sh 'sed -i '/baseURL/s/#//g' config.toml'
        sh 'sed -i '/googleAnalytics/s/#//g' config.toml'
        sh './hugo'
        sh 'rsync -a --delete public/ /var/www/docs.tugboat.qa/'
      }
    }
  }

  post {
    unsuccessful {
      mail to: tugboat@lullabot.com, subject: 'Failed'
    }
  }
}
