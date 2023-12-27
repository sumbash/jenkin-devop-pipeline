pipeline{
    agent{
        label "jenkins-agent"
    }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
 
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/sumbash/jenkin-devop-pipeline'
            }

        }

               stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

        }

               stage("Test Application"){
            steps {
                sh "mvn test"
            }

        }
                stage("sonarqube analysis"){
            steps {
                script {
                withSonarQubeEnv(credentialsId : "jenkins-sonarqube-token1") {
                sh "mvn sonar:sonar"
            }
          }
        }
    }

     stage("quality gate"){
            steps {
                script {
                    waitForQualityGate abortpipeline: false, credentialsId : "jenkins-sonarqube-token1"
          
          }
        }
    }
}

}