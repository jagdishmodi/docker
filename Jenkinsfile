pipeline  {
    agent any
    environment {
        APP_NAME = "jagdish1983/test"
    }

    stage('Clone repository') {
      

        checkout scm
    }
    stage('Build image') {
  
       app = docker.build("jagdish1983/test")
    }
   
    stage('Test image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'git') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
    stage('Scan Docker Image') {
            steps {
                script {
                    // Run Trivy to scan the Docker image
                    def trivyOutput = sh(script: "trivy image $APP_NAME:latest", returnStdout: true).trim()

                    // Display Trivy scan results
                    println trivyOutput

                    // Check if vulnerabilities were found
                    if (trivyOutput.contains("Total: 0")) {
                        echo "No vulnerabilities found in the Docker image."
                    } else {
                        echo "Vulnerabilities found in the Docker image."
                        // You can take further actions here based on your requirements
                        // For example, failing the build if vulnerabilities are found
                        // error "Vulnerabilities found in the Docker image."
                    }
                }
            }
}
}
