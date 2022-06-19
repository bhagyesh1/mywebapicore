pipeline {
    agent any
    stages {
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
            steps   {
                script{
                     withSonarQubeEnv() {
                        bat "dotnet tool install --global dotnet-sonarscanner"
                        bat "{dotnet sonarscanner begin /k:"MyWebAPICore" /d:sonar.host.url="http://127.0.0.1:9000"  /d:sonar.login="sqp_da8ca3b35d42aeae75bc689b2c3ba0ded8d459d3"}"
                        bat "dotnet build"
                        bat dotnet sonarscanner end /d:sonar.login="sqp_da8ca3b35d42aeae75bc689b2c3ba0ded8d459d3"
                     
                    }
                }
            }
        }
            stage('Restore packages') {
                steps {
                    bat "dotnet restore ${workspace}\\MyWebAPICore.csproj"
                }
            }
            stage('Clean'){
                steps{
                    bat "dotnet clean ${workspace}\\MyWebAPICore.csproj"
                }
            }
            stage('Build'){
                steps{
                    bat "dotnet build ${workspace}\\MyWebAPICore.csproj --configuration Release"
                }
            }

            stage('Test: Unit Test'){
                steps {
                    bat "dotnet test ${workspace}\\MyWebAPICore.csproj"
                }
            }
            
            stage('Test: Integration Test'){
                steps {
                    bat "dotnet test ${workspace}\\MyWebAPICore.csproj"
                }
            }

            stage('Publish'){
                steps{
                    bat "dotnet publish ${workspace}\\MyWebAPICore.csproj"
                }
            }

    }
}