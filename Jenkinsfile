pipeline {
    docker {
        image 'maven:3.8-openjdk-17'
        args '-v /root/.m2:/root/.m2 -v /root/.ssh:/root/.ssh'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
    }
}