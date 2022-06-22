pipeline {
    agent any 
    tools {
  maven 'M2_HOME'
 }
     environment {
        registry = "okip/fullcicd"
        registryCredential = 'docker_user'
     }
     stages {
        stage ('Build') {
            steps {
                sh 'mvn clean'
                sh 'mvn install'
                sh 'mvn package'
            }
        }
        stage ("Test") {
            steps {
                echo "TEST STAGE"
                sh 'mvn test'
            }
        }
        stage ('Build') {
            steps {
                script {
                    docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage ('Deploy to Docker') {
            steps {
                sh 'docker push okip/fullcicd + ":$BUILD_NUMBER" '
            }
        }
     }

}
