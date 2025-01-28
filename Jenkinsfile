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
                    def dockerCmd = 'docker run -p 8080:8080 -d ahmedabdelnasermohamed2/demo-app:1.0'
                    sshagent(['ec2-server-key']) {
                        sh "ssh StrictHostKeyChecking=no ec2-user@157.175.224.143 ${dockerCmd}"
}               }
            }
        }
    }
}

