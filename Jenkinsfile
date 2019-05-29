pipeline {
  agent any

  tools {
      maven 'maven'
  }

  stages {
      stage ('Git clone') {
          steps {
              git 'https://github.com/boxfuse/boxfuse-sample-java-war-hello.git'
          }
      }

      stage ('Maven build') {
          steps {
              sh 'mvn clean package'
          }
      }

      stage ('Building and pushing docker image') {
          steps {
              sh """
                echo 'FROM tomcat:8.0' >> Dockerfile
                echo 'COPY ./target/hello-1.0.war /usr/local/tomcat/webapps/' >> Dockerfile
                echo 'EXPOSE 8080' >> Dockerfile
                docker build . -t boxfuse
                docker tag boxfuse serg2/boxfuse-demo
                docker push serg2/boxfuse-demo
              """
          }
      }

      stage ('Deploy') {
          steps {
              sh 'docker run -d -p 8083:8080 serg2/boxfuse-demo'
          }
      }
    }
}