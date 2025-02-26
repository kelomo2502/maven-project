pipeline {
    agent {label "dev-server"}
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
                    agent { label "dev-server" }
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
            options { beforeAgent true }
            steps {
                dir("/var/www/html") {
                    unstash "maven-build"
                }
                sh """
                    cd /var/www/html/
                    jar -xvf webapp.war
                """
            }
        }
    }
}


// pipeline
// {

// agent {
//   label 'DevServer'
// }

// parameters {
//     choice choices: ['dev', 'prod'], name: 'select_environment'
// }

// environment{
//     NAME = "piyush"
// }
// tools {
//   maven 'mymaven'
// }

// stages{

//     stage('build')
//     {
//         steps {
//             script{
//                 file = load "script.groovy"
//                 file.hello()
//             }
//             sh 'mvn clean package -DskipTests=true'
           
//         }

        

//     }

//     stage('test')
//     { 
//         parallel {
//             stage('testA')
//             {
//                 agent { label 'DevServer' }
//                 steps{
//                     echo " This is test A"
//                     sh "mvn test"
//                 }
                
//             }
//             stage('testB')
//             {
//                 agent { label 'DevServer' }
//                 steps{
//                 echo "this is test B"
//                 sh "mvn test"
//                 }
//             }
//         }
//         post {
//         success {
//              dir("webapp/target/")
//             {
//             stash name: "maven-build", includes: "*.war"
//                  }
//                  }
//             }

//     }

//     stage('deploy_dev')
//     {
//         when { expression {params.select_environment == 'dev'}
//         beforeAgent true}
//         agent { label 'DevServer' }
//         steps
//         {
//             dir("/var/www/html")
//             {
//                 unstash "maven-build"
//             }
//             sh """
//             cd /var/www/html/
//             jar -xvf webapp.war
//             """
//         }
//     }

//     stage('deploy_prod')
//     {
//       when { expression {params.select_environment == 'prod'}
//         beforeAgent true}
//         agent { label 'ProdServer' }
//         steps
//         {
//              timeout(time:5, unit:'DAYS'){
//                 input message: 'Deployment approved?'
//              }
//             dir("/var/www/html")
//             {
//                 unstash "maven-build"
//             }
//             sh """
//             cd /var/www/html/
//             jar -xvf webapp.war
//             """
//         }  
//     }

   

    
// }

// }
