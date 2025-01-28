def gv

pipeline {
    agent any
    tools {
        maven 'maven-3.9' 
    }
    stages {
        stage('increment version') {
            steps {
                script {
                    echo 'incrementing app version'
                    sh 'mvn build-helper:parse-version versions:set -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }
        stage('build app') {
            steps {
                script {
                    echo 'building the application...'
                    sh 'mvn clean package'
                }
            }
        }
        stage('build image') {
            steps {
                script {
                    echo "building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'PASS', usernameVariable: 'USER')]){
                        sh "docker build -t nicolebrinza/twn-demo-app:${IMAGE_NAME} ."
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh "docker push nicolebrinza/twn-demo-app:${IMAGE_NAME}"
                    } // Added missing closing bracket
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    echo 'deploying docker image'
                    def dockerComposeCmd = "docker-compose -f docker-compose.yaml up --detach"
                    sshagent(['ec2-server-key']){
                        sh "scp docker-compose.yaml ec2-user@157.175.224.143 /home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@157.175.224.143 ${dockerComposeCmd}"
                    }
                }
            }
        }      
    }
}
