#!/usr/bin/env groovy

pipeline {
    agent none
    tools {
        maven 'maven-3.9'
    }
    stages {
        stage('build jar') {
            steps {
                script {
                    echo "Building the application..."
                    sh 'mvn package'
                }
            }
        }
        stage('build image') {
            steps {
                script {
                    echo "Building the docker image"
                    sh 'mvn package'
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')])
                    sh 'docker built -t ahmedabdelnasermohamed2/my-repo:jma-2.0 .'
                    sh "echo $PASSWORD | docker login -u $USERNAME --password-stdin"
                    sh 'docker push ahmedabdelnasermohamed2/demo-app:jma-2.0'
                }
            }
        }
        stage('test') {
            steps {
                script {
                    echo "Testing the application..."
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    echo "Deploying the application..."
                }
            }
        }
    }
}
