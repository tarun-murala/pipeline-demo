pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage("checkout") {
            steps {
                snDevOpsStep()
                snDevOpsChange()
                echo "Building" 
                checkout scm
            }
        }

        stage("build") {
            steps {
                snDevOpsStep()
                snDevOpsChange()
                echo "Building" 
                sh "mvn clean install -DskipTests=true"
            }
        }

        stage('unit-tests') {
            steps {
                snDevOpsStep()
                snDevOpsChange()
                echo "Unit Test"
                sh "mvn test"
                sleep 5
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml' 
                }
          }
        }
        
        stage('performance-tests') {
            steps {
                snDevOpsStep ()
                snDevOpsChange()
                echo "Testing"
                sh "mvn test"
                sleep 5
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml' 
                }
          }
        }

        stage('integration-tests') {
            steps {
                snDevOpsStep ()
                snDevOpsChange()
                echo "Testing"
                sh "mvn test"
                sleep 5
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml' 
                }
          }
        }

        stage("deploy") {
            stages{
                stage('deploy to dev') {
                    when{
                        branch 'dev'
                    }
                    steps{
                        snDevOpsStep ()
                        echo "deploy in UAT"
                        snDevOpsChange()
                    }
                }
                stage('deploy to prod') {
                    when {
                        branch 'master'
                    }
                    steps{
                        snDevOpsStep ()
                        echo "deploy in prod"
                        snDevOpsChange()
                    }
                }
            }
        }

        stage("final-smoke-test") {
            steps {
                snDevOpsStep ()
                snDevOpsChange()
                echo "Testing"
                sh "mvn test"
                sleep 5
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml' 
                }
          }
        }
    }

}