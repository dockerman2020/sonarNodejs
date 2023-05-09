pipeline {
  agent { 
      kubernetes {
            yaml '''
                apiVersion: v1
                kind: Pod
                spec:
                    containers:
                    - name: node
                      image: node
                      command:
                      - cat
                      tty: true
                '''
           }
	 }
  options {
    buildDiscarder(logRotator(numToKeepStr: '15'))
  }
    stages{
        stage("clone") {
            steps{
                git branch: 'main', url: 'https://github.com/dockerman2020/sonarNodejs.git'
            }
        }
        stage("sonarqube analysis") {
            steps{
            nodejs(nodeJSInstallationName: 'SonarNodeJS') {
                sh "npm install"
                withSonarQubeEnv('SonarQube'){
                    sh "npm install --save-dev mocha chai"
                    sh "npm run test"
                    sh "npm run coverage-lcov"
                    sh "npm install sonar-scanner"
                    sh "npm run sonar"
                    }
                }
            }
        }
    }
}
