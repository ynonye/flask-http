node('docker-slave-general') { 
  def DockerImage = "webserver:v1.0"
  
  stage('Pre') { // Run pre-build steps
    cleanWs()
    sh "docker rm -f webserver || true"
  }
  
  stage('Git') { // Get code from GitLab repository
    git branch: 'master',
      url: 'https://github.com/MadDamDam/flask-http.git'
  }
  
  stage('Build') { // Run the docker build
    sh "docker build --tag ${DockerImage} ."
  }
  
  stage('Run') { // Push the image to repository
    sh "docker run -d --name webserver --rm -p 8081:5000 flask-ex:v1.0; sleep 5"
  }
  
  stage('Run') { // Push the image to repository
    def dockerOutput = sh (
        script: 'curl http://172.17.0.1:8081/goaway',
        returnStdout: true
        ).trim()
    sh "docker rm -f webserver"
    
    if ( dockerOutput == 'GO AWAY!' ) {
        currentBuild.result = 'SUCCESS'
    } else {
        currentBuild.result = 'FAILURE'
        sh "echo Webserver returned ${dockerOutput}"
    }
    return
  }
}
