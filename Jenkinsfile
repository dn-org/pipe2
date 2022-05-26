

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
                withCredentials([usernamePassword(credentialsId: 'jfrog-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh      'docker build --network=host -t npm_proj:$BUILD_ID .'
                    //sh      'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock --network=host aquasec/trivy mongodbadminui:$BUILD_ID'  
                    echo    'deploy package to artifactory'             
                    sh      'docker login scribesecuriy.jfrog.io -u ${USERNAME} -p ${PASSWORD}'
                    sh      'docker tag npm_proj:$BUILD_ID scribesecuriy.jfrog.io/test-repo-docker/npm_proj:$BUILD_ID'  
                    sh      'docker tag npm_proj:$BUILD_ID scribesecuriy.jfrog.io/test-repo-docker/npm_proj:latest'  
                    sh      'docker push scribesecuriy.jfrog.io/test-repo-docker/npm_proj:$BUILD_ID' 
                    sh      'docker push scribesecuriy.jfrog.io/test-repo-docker/npm_proj:latest'
                    
                }
            }
        }
    }
  }
}

