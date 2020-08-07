pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        dockerTool "docker"
    }

    stages {
        stage('VCS') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/indrarahul2013/hello-world-webapp'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    withDockerRegistry(
                        credentialsId: 'b776e137-e3f0-41f2-beba-f3fe0cc5cb26',
                        toolName: 'docker') {
                        
                        // Build and Push
                        def echoServerImage = docker.build("indrarahul2018/hello-world-webapp");
                        echoServerImage.push();
                    }
                }
            }
        }
    }
    post {
        def status = "X"
        success {
            echo "success"
        }
        failure {
            echo "fail"
        }

        // httpRequest contentType: 'TEXT_PLAIN', customHeaders: [[maskValue: false, name: '', value: '']], httpMode: 'POST', requestBody: '''Project Name: hello-world-webapp
        // Build Commit: asdasdsa
        // Build Status: ''' + status, responseHandle: 'NONE', url: 'https://api.flock.com/hooks/sendMessage/55c8786a-8494-4233-9659-ce928d5ebd1b', wrapAsMultipart: false
    }

    
}
