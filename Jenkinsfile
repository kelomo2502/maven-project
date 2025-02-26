pipeline {
    agent any
    
    parameters {
        choice(
            choices: ['dev-server', 'prod-server'], 
            name: 'select_environment'
        )
    }
    
    environment {
        NAME = "gbenga"
    }
    
    tools {
        maven "gb-maven"
    }
    
    stages {
        stage("Build") {
            steps {
                sh "mvn clean package -DskipTests=true"
            }
            post {
                success {
                    archiveArtifacts artifacts: "**/target/*.war"
                }
            }
        }
        
        stage("Test") {
            parallel {
                stage("Test A") {
                    agent { label "dev-server" }
                    steps {
                        echo "This is test A"
                        sh "mvn test"
                    }
                }
                stage("Test B") {
                    agent { label "prod-server" }
                    steps {
                        echo "This is test B"
                        sh "mvn test"
                    }
                }
            }
            post {
                success {
                    dir("webapp/target/") {
                        stash name: "maven-build", includes: "*.war"
                    }
                }
            }
        }
        
        stage("Deploy to Dev") {
            when { expression { params.select_environment == "dev-server" } }
            agent { label "dev-server" }
            steps {
                dir("/tmp/deploy") {
                    unstash "maven-build"
                }
                sh """
                    sudo cp /tmp/deploy/webapp.war /var/www/html/webapp.war
                    cd /var/www/html
                    sudo jar -xvf webapp.war
                """
            }
        }
    }
}
