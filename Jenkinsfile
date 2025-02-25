pipeline {
    agent { label "prod-server" }
    
    tools {
        maven "gb-maven"
    }
    
    stages {
        stage("build") {
            steps {
                sh "mvn clean package"
            }
            post {
                success {
                    archiveArtifacts artifacts: "**/target/*.war"
                }
            }
        }
    }
}
