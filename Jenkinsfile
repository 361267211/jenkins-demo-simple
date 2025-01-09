pipeline {
    environment {
        DOCKER_IMAGE = 'myrepo/myjen'        // Docker 镜像的名称
        DOCKER_TAG = 'v1.0.0'                // Docker 镜像的标签
    }
    agent {
            docker {
                image 'maven:3.8-openjdk-17'
                args '-v /root/.m2:/root/.m2 -v /root/.ssh:/root/.ssh'
            }
        }
    stages {
//         stage('Build') {
//             steps {
//                 sh 'mvn -B -DskipTests clean package'
//             }
//         }
//         stage('Package Docker Image') {
//             steps {
//                 script {
//                     sh 'mvn clean package dockerfile:build -DskipTests'
//                 }
//             }
//         }
        stage('Push Docker Image') {
            steps {
                script {
                    sh 'docker ps'  // 推送镜像到仓库
                   // sh 'docker push $DOCKER_IMAGE:$DOCKER_TAG'  // 推送镜像到仓库

                }
            }
        }
    }
}
