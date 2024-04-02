#!/usr/bin/env groovy

import groovy.transform.Field

@Field
String DOCKER_USER_REF = 'vandat-docker-hub'
@Field
String SSH_ID_REF = 'ssh-credentials-id'

pipeline {
    agent any

    tools {
        dockerTool 'docker'
    }

    stages {
        stage("build and test") {
            steps {
                sh "ls -la"
                sh "docker build -t gaumeodathanh/devops-mgm-training-todo-app:0.0.2 ."
                sh "docker images"
            }
        }
        stage("Docker login and push docker image") {
            steps {
                withBuildConfiguration {
                    sh 'docker login --username ${repository_username} --password ${repository_password}'
                    sh "docker push gaumeodathanh/devops-mgm-training-todo-app:0.0.2"
                }
            }
        }
        stage("deploy") {
            steps {
                withBuildConfiguration {
                    sshagent(credentials: [SSH_ID_REF]) {
                        sh '''
                            ssh -o StrictHostKeyChecking=no root@ec2-18-143-167-76.ap-southeast-1.compute.amazonaws.com 
                            "docker rm -f 93a7eabad6380d0a13bc30d6e290d3a2c7d25b8140f017bdff2debd6a1036181"
                            "docker run --detach --name tvandat-todo-app -p 9603:8000 gaumeodathanh/devops-mgm-training-todo-app:0.0.2"
                        '''
                    }
                }
            }
        }
    }
}

void withBuildConfiguration(Closure body) {
    withCredentials([usernamePassword(credentialsId: DOCKER_USER_REF, usernameVariable: 'repository_username', passwordVariable: 'repository_password')]) {
        body()
    }
}
