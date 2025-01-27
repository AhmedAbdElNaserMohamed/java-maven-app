#!/usr/bin/env groovy
library identifier: 'jenkins-shared-library@master', retriever: modernSCM([$class: 'GitSCMSource', remote: 'https://gitlab.com/TWN1515/jenkins-shared-library.git', credentialsID: 'gitlab-credentials'])

def gv

pipeline {   
    agent any
    tools {
        maven "maven-3.9"
    }

    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }

        stage("build jar") {
            steps {
                script {
                    buildJar()
                }
            }
        }

        stage("build and push image") {
            steps {
                script {
                    buildImage 'nicolebrinza/twn-demo-app:jma-3.0'
                    dockerLogin()
                    dockerPush 'nicolebrinza/twn-demo-app:jma-3.0'
                }
            }
        }
        
        stage("deploy") {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }               
    }
}
