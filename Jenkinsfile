node {
  stage('SCM') {
    git 'https://github.com/bhagyesh1/mywebapicore.git'
  }
  stage('Build + SonarQube analysis') {
    def sqScannerMsBuildHome = tool 'SonarQubeScanner'
    withSonarQubeEnv('My SonarQube Server') {
      bat "${sqScannerMsBuildHome}\\SonarQube.Scanner.MSBuild.exe begin /k:sqp_da8ca3b35d42aeae75bc689b2c3ba0ded8d459d3"
      bat 'MSBuild.exe /t:Rebuild'
      bat "${sqScannerMsBuildHome}\\SonarQube.Scanner.MSBuild.exe end"
    }
  }
}