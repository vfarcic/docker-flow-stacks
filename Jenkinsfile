import java.text.SimpleDateFormat

pipeline {
  agent {
    label "test"
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '2'))
  }
  triggers {
    cron('H H * * 0')
  }
  stages {
    stage("init") {
      steps {
        script {
          def dateFormat = new SimpleDateFormat("yy.MM.dd")
          currentBuild.displayName = dateFormat.format(new Date()) + "-" + env.BUILD_NUMBER
        }
        withCredentials([usernamePassword(
          credentialsId: "docker",
          usernameVariable: "USER",
          passwordVariable: "PASS"
        )]) {
          sh "docker login -u $USER -p $PASS"
        }
      }
    }
    stage("jenkins") {
      steps {
        sh "cd jenkins && docker image build --no-cache -t vfarcic/jenkins ."
        sh "docker image tag vfarcic/jenkins vfarcic/jenkins:${currentBuild.displayName}"
        sh "docker image push vfarcic/jenkins:${currentBuild.displayName}"
        sh "docker image push vfarcic/jenkins"
      }
    }
    stage("compose") {
      steps {
        sh "cd docker/compose && docker image build --no-cache -t vfarcic/compose ."
        sh "docker image push vfarcic/compose"
      }
    }
  }
  post {
    always {
      sh "docker system prune -f"
    }
    failure {
      slackSend(
        color: "danger",
        message: "${env.JOB_NAME} failed: ${env.RUN_DISPLAY_URL}"
      )
    }
  }
}
