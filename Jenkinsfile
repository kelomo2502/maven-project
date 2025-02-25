pipeline {
    agent { label "prod-server" }

    parameters {
            string defaultValue: 'oyewunmi', name: 'Last_Name'
    }


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
                echo "Hello $NAME ${params.Last_Name}"
            }
            post {
                success {
                    archiveArtifacts artifacts: "**/target/*.war"
                }
            }
        }
        stage("test") {
                parallel {
                    stage("Test A"){
                        steps{
                            echo "This is test A"
                        }
                    }
                    stage("Test B"){
                        steps{
                            echo "This is test B"
                        }
                    }
                }
                post {
                success {
                    archiveArtifacts artifacts: "**/target/*.war"
                }
            }
        }
    }
}
