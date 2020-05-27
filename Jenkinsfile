node {
  def myGradleContainer = docker.image('gradle:jdk8')
  myGradleContainer.pull()
  
  try {
    stage('prep') {
      checkout scm
    }
    stage('test') {
       myGradleContainer.inside("-v ${env.HOME}/.gradle:/home/gradle/.gradle") {
         sh 'cd complete && gradle test'
       }
    }
    stage('run') {
       myGradleContainer.inside("-v ${env.HOME}/.gradle:/home/gradle/.gradle") {
         sh 'cd complete && gradle run'
       }
    }
  } catch(e) {
    // mark build as failed
    currentBuild.result = "FAILURE";

    // send slack notification
    slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

    // throw the error
    throw e;
  }
}
