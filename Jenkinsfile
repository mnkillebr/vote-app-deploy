node {
  def app

  stage('Clone repository') {
    checkout scm
  }

  stage('Update GIT') {
    script {
      catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
        withCredentials([usernamePassword(credentialsId: 'mnkillebr', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
          sh "git config user.email coach.mkillebrew@gmail.com"
          sh "git config user.name mnkillebr"
          sh "cat vote-ui-deployment.yaml"
          sh "sed -i 's+killebrew4dev/vote.*+killebrew4dev/vote:${DOCKERTAG}+g' vote-ui-deployment.yaml"
          sh "cat vote-ui-deployment.yaml"
          sh "git add ."
          sh "git commit -m 'Done by Jenkins Job deployment: ${env.BUILD_NUMBER}'"
          sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/vote-app-deploy.git HEAD:master"
        }
      }
    }
  }

  stage('Trigger deployment') {
    def dockerTag = params.DOCKERTAG ?: env.DOCKERTAG ?: DOCKERTAG
    echo "Triggering deployment with DOCKERTAG: ${dockerTag}"
    build job: 'deployment', parameters: [string(name: 'DOCKERTAG', value: "${dockerTag}")]
  }
}