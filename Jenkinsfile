pipeline {
    environment {
        DOCKER_IMAGE = 'your-docker-repository/demo'        // Docker 镜像的名称
        DOCKER_TAG = '0.0.1-SNAPSHOT'                        // Docker 镜像的标签
        REMOTE_SERVER = 'root@192.168.31.82'                // 远程服务器地址
        CONTAINER_NAME = 'my-jen-demo'                      // 运行容器的名称
    }
    agent {
            docker {
                label 'docker'
                image 'maven:3.8-openjdk-17'
                args '-v /root/.m2:/root/.m2 -v /root/.ssh:/root/.ssh'
            }
        }
    stages {
        stage('Remote SSH') {
            steps {
                script {
                    // 执行远程命令
                    sh 'ssh -o StrictHostKeyChecking=no $REMOTE_SERVER "echo hello"'
                }
            }
        }

        stage('Remove Container') {
                    steps {
                        script {
                            //先删除容器，再删镜像(不删会一直增加新镜像，占用大量存储)
                            sh 'docker stop $CONTAINER_NAME && docker rm $CONTAINER_NAME || true'
                        }
                    }
                }

        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Package Docker Image') {
            steps {
                script {
                    sh 'mvn dockerfile:build -DskipTests'
                }
            }
        }
//有本地的hub 库时才使用这一步
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
                    sh 'docker run -d --name $CONTAINER_NAME -p 8090:8080 $DOCKER_IMAGE:$DOCKER_TAG'

                }
            }
        }

         stage('Remos None Image') {
            steps {
                script {
                    // 这里直接运行宿主机中的 Docker 镜像
                    sh 'docker images | grep "<none>" | awk "{print $3}" | xargs docker rmi || true'
                }
            }
        }
    }
}
