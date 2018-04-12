node('docker') {
  def app
  def commit_id

  stage('initialize') {
    git([url: 'https://github.com/ranjithkumar-s/node-js-sample.git', branch: 'master'])

    sh 'git rev-parse HEAD > .git/commit-id'
    commit_id = readFile('.git/commit-id').trim()
  }

  stage('build') {
    docker.withRegistry('http://192.168.10.102:5000') {
      app = docker.build('helloworld')

      stage('publish') {
        app.push(commit_id)
      }
    }
  }

  stage('deploy') {
    env.TAG = "${commit_id}"
    sh 'rancher --env ${ENVID} up -p -d --upgrade -s app helloworld'
  }
  
  // stage('remove local images') {
        // remove docker images
        // sh("docker rmi -f 192.168.10.102:5000/helloworld:latest || :")
        // sh("docker rmi -f 192.168.10.102:5000/helloworld:${commit_id} || :")
        // sh("docker rmi -f helloworld:${commit_id} || :")
   // }
  
}
