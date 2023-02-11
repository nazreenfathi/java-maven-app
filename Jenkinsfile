pipeline{
    agent any

    tools {
        maven 'Maven-3.9.0'       
    }

    stages{
        stage('git Checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'git credentials', url: 'https://github.com/nazreenfathi/java-maven-app.git']])
            }
            
        }

        stage('maven build'){
            steps{
                sh 'mvn clean install'
            }
        }
        
        stage('sonarqube analysis'){
            steps{
                withSonarQubeEnv("SonarQube") {
                        sh "${tool("sonarQube")}/bin/sonar-scanner \
                        -Dsonar.projectKey=java-maven-app \
                        -Dsonar.java.binaries=target \
                        -Dsonar.host.url=http://44.203.21.133:9000/ \
                        -Dsonar.login=sqp_ec96bdff8838cfb5997b59ddbb5bf710d3a96636"
                    }
            }
        }
        stage('upload artifacts in nexus'){
            steps{
                sh 'mvn -s settings.xml clean deploy'
            }
        }
    }

}