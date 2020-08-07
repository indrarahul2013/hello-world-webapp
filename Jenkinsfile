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
        success {
            echo "success"
            
            script{
                def scmVars = checkout([
        $class: 'GitSCM',
        ...
      ])
      echo "${scmVars.GIT_COMMIT}"
            echo "scmVars.GIT_COMMIT"
                httpRequest contentType: 'APPLICATION_JSON', httpMode: 'POST', requestBody: '{"text":"Project Name: hello-world-webapp\nBuild Commit: ${scmVars.GIT_COMMIT}\nBuild Status: Chal Gya Bhai!!"}', responseHandle: 'NONE', url: 'https://api.flock.com/hooks/sendMessage/a9705b01-3454-4aea-a87d-1fdd0e8d98c0', wrapAsMultipart: false
            }
            
        }
        failure {
            echo "fail"
            httpRequest contentType: 'APPLICATION_JSON', httpMode: 'POST', requestBody: '{"text":"Project Name: hello-world-webapp\nBuild Commit: ${scmVars.GIT_COMMIT}\nBuild Status: Fat Gya Bhai!!"}', responseHandle: 'NONE', url: 'https://api.flock.com/hooks/sendMessage/a9705b01-3454-4aea-a87d-1fdd0e8d98c0', wrapAsMultipart: false
        }
    }

    
}
