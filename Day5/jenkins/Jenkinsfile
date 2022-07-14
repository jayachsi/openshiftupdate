#!/usr/bin/env groovy
pipeline {
    agent {
        label 'docker-slave'
    }
    stages {
        stage('Compile') {
            steps {
                script {
                    sh "mvn clean compile"
                }
            }
        }
        stage('Unit Test') {
            steps {
                script {
                    sh "mvn test"
                }
            }
        }
        stage('package') {
            steps {
                script {
                    sh "mvn package"
                }
            }
        }
        stage('Install') {
            steps {
                script {
                    sh "mvn install"
                }
            }
        }
    }
}
