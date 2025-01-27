pipeline {
    agent any
    stages {
        stage('test') {
            steps {
               script {
                   echo 'testing the app'
                   echo "Executing pipeline for branch $BRANCH_NAME"
               }
            }
        }
        stage('build') {
            when {
                expression {
                    branch_name == "master"
                }
            }
            steps {
               script {
                   echo 'building the app'
                
               }
            }
        }
        stage('deploy') {
            when {
                expression {
                    branch_name == "master"
                }
            }
            steps {
                script {
                   echo 'deploying the app'
               }
            }
        }
    }
}

