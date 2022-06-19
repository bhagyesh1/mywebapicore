pipeline {
    agent any
    stages {
        /*stage('SCM') {
            steps {
                // Clean before build
                cleanWs()
                git url: 'https://github.com/bhagyesh1/mywebapicore.git'
            }
        }*/

        stage('DEV/BETA/MAIN Branch Building image & run') {
           when {
                branch 'dev'
            }
            steps{
                script{
                    echo "Process is starting!"
                }
            }
        }
        stage('build && SonarQube analysis') {
            environment {
            scannerHome = tool 'SonarQubeScanner'
        }
            steps {
                script{
                     withSonarQubeEnv("sonarqube") {
                     bat "${tool("SonarQubeScanner")}/bin/sonar-scanner \
                        -Dsonar.projectKey=MyWebAPICore \
                        -Dsonar.sources=. \
                        -Dsonar.css.node=. \
                        -Dsonar.host.url=http://127.0.0.1:9000 \
                        -Dsonar.login=sqp_da8ca3b35d42aeae75bc689b2c3ba0ded8d459d3"
                    }
                }
            }
        }
            stage('Restore packages') {
                steps {
                    bat "dotnet restore ${workspace}\\mywebapicore\\MyWebAPICore.csproj"
                }
            }
            stage('Clean'){
                steps{
                    bat "dotnet clean ${workspace}\\mywebapicore\\MyWebAPICore.csproj"
                }
            }
            stage('Build'){
                steps{
                    bat "dotnet build ${workspace}\\mywebapicore\\MyWebAPICore.csproj --configuration Release"
                }
            }

            stage('Test: Unit Test'){
                steps {
                    bat "dotnet test ${workspace}\\mywebapicore\\MyWebAPICore.csproj"
                }
            }
            
            stage('Test: Integration Test'){
                steps {
                    bat "dotnet test ${workspace}\\mywebapicore\\MyWebAPICore.csproj"
                }
            }

            stage('Publish'){
                steps{
                    bat "dotnet publish ${workspace}\\mywebapicore\\MyWebAPICore.csproj"
                }
            }

    }
}