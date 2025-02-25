pipeline {
    agent { label "prod-server" }

    environment {
        NAME = "gbenga"
    }
    
    tools {
        maven "gb-maven"
    }
    
    stages {
        stage("build") {
            steps {
                sh "mvn clean package"
                echo "Hello $NAME"
            }
            post {
                success {
                    archiveArtifacts artifacts: "**/target/*.war"
                }
            }
        }
    }
}
