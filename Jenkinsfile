#!groovy
pipeline {
  agent none
  stages {
    stage('Maven Install') {
      agent {
        docker {
          image 'maven:3.9-eclipse-temurin-25'
          args '-v maven-repo:/root/.m2'
          reuseNode true
        }
      }
      steps {
        retry(3) {
          sh 'mvn -B -Dmaven.wagon.http.retryHandler.count=5 clean install -DskipTests'
        }
      }
    }
    stage('Docker Build') {
      agent any
      steps {
        sh 'docker build -t josem1715/spring-petclinic:gestion-udem-jenkins .'
      }
    }
    stage('Docker Push') {
      agent any
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push josem1715/spring-petclinic:gestion-udem-jenkins'
        }
      }
    }
  }
}
