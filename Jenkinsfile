

pipeline {
  agent {
    kubernetes {
      cloud 'kubernetes'
      label 'scribe-jenkins-namespace'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
labels: 
  component: ci
metadata: ~
spec: 
  containers: 
    - name: agent
      image: scribesecuriy.jfrog.io/scribe-docker-public-local/agent:latest
      command:
      - cat
      tty: true
      env:
      - name: CONTAINER_ENV_VAR
        value: agent
    - 
      command: 
        - cat
      image: "openjdk:11"
      name: jdk11
      tty: true
    - 
      command: 
        - cat
      image: "bitnami/kubectl:latest"
      name: kubectl
      tty: true
    - 
      command: 
        - cat
      image: "node:16"
      name: npm
      tty: true
    - 
      command: 
        - cat
      image: "docker:latest"
      name: docker
      tty: true
      volumeMounts: 
        - 
          mountPath: /var/run/docker.sock
          name: docker-sock
  serviceAccountName: jenkins
  volumes: 
    - 
      hostPath: 
        path: /var/run/docker.sock
      name: docker-sock
"""
}
   }

  parameters {
      string(defaultValue: 'https://api.dev.scribesecurity.com/guy-test',
        name: 'SCRIBE_URL',
        trim: true)
  }
  stages{
    stage('Build, scan & Push') {
        steps {
            container('docker') {
                //withCredentials([usernamePassword(credentialsId: 'jfrog-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    
                    sh 'docker ps '
                    
                //}
            }
        }
    }
  }
}

