// pipeline {
//     // agent { label "prod-server" }

//     parameters {
//               choice choices: ['dev-server', 'prod-server'], name: 'select_environment'
//     }


//     environment {
//         NAME = "gbenga"
//     }
    
//     tools {
//         maven "gb-maven"
//     }
    
//     stages {
//         stage("build") {
//             steps {
//                 sh "mvn clean package -DskipTests=true"
//             }
//             post {
//                 success {
//                     archiveArtifacts artifacts: "**/target/*.war"
//                 }
//             }
//         }
//         stage("test") {
//                 parallel {
//                     stage("Test A"){
//                         agent {label "dev-server"}
//                         steps{
//                             echo "This is test A"
//                             sh "mvn test"
//                         }
//                     }
//                     stage("Test B"){
//                         agent {label "prod-server"}
//                         steps{
//                             echo "This is test B"
//                             sh "mvn test"
//                         }
//                     }
//                 }
//                 post {
//                 success {
//                     dir("webapp/target/"){
//                         stash name: "maven-build", includes: "*.war" {

//                         }
//                     }
//                 }
//             }
//         }
//     }
//     stage("deploy-dev") {
//         when {expression {params.select_environment == "dev-server"} beforeAgent true}
//         agen {label "dev-server"}
//         steps {
//             dir("/var/www/html") {
//                 unstash "maven-build"
//             }
//             sh """
//                 cd /var/www/html
//                 jar -xvf webapp.war
//             """
//         }
//     }
// }


pipeline {

    parameters {
        choice choices: ['dev-server', 'prod-server'], name: 'select_environment'
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
                sh "mvn clean package -DskipTests=true"
            }
            post {
                success {
                    archiveArtifacts artifacts: "**/target/*.war"
                }
            }
        }

        stage("test") {
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

        stage("deploy-dev") {
            when { expression { params.select_environment == "dev-server" } }
            agent { label "dev-server" }
            steps {
                dir("/var/www/html") {
                    unstash "maven-build"
                }
                sh """
                    cd /var/www/html
                    jar -xvf webapp.war
                """
            }
        }
    }
}
