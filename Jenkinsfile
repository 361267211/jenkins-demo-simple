pipeline {
    environment {
        DOCKER_IMAGE = 'your-docker-repository/demo'        // Docker 镜像的名称
        DOCKER_TAG = '0.0.1-SNAPSHOT'                        // Docker 镜像的标签
    }
    agent {
            docker {
                label 'docker'
                image 'maven:3.8-openjdk-17'
                args '-v /root/.m2:/root/.m2 -v /root/.ssh:/root/.ssh'
            }
        }
    stages {

        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Package Docker Image') {
            steps {
                script {
                    sh 'mvn clean package dockerfile:build -DskipTests'
                }
            }
        }

//         stage('Push Docker Image') {
//             steps {
//                 script {
//                    // sh 'docker ps'  // 推送镜像到仓库
//                     sh 'docker push $DOCKER_IMAGE:$DOCKER_TAG'  // 推送镜像到仓库
//
//                 }
//             }
//         }
         stage('Run Docker Container') {
            steps {
                script {
                    // 这里直接运行宿主机中的 Docker 镜像
                    sh "docker stop $DOCKER_IMAGE:$DOCKER_TAG && docker rm $DOCKER_IMAGE:$DOCKER_TAG"
                    sh "docker run -d --name myjen-container -p 8090:8080 $DOCKER_IMAGE:$DOCKER_TAG"

                }
            }
        }
    }
}
