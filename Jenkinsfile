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
        stage ('Build Docker Image ') {
            steps {
                script {
                    docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage ('Deploy to Docker') {
            steps {
                script{
                     docker.withRegistry( '', registryCredential ) {
                     sh 'docker push okip/fullcicd' + ":$BUILD_NUMBER"
                    }
                } 
            }
        }
        stage ('Deploy to EKS') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'KubenetesID', namespace: '', serverUrl: '') {
                  sh 'kubectl apply -f web-app.yml'
              }
            }
        }
     }

}
