node{
    
   
        stage('Maven Compile'){
            def mvnHome = tool name: 'maven3', type: 'maven'
            sh "${mvnHome}/bin/mvn clean package"
            sh "mv target/myweb*.war target/myapp.war"
        }
        
        stage('Build Docker Image'){
            sh 'docker build -t ajay/myapp.0.1 .'
        }
        
        stage('Docker Deployment'){
            // Stop and remove any existing container with the name "mywebapp" if it exists
            sh 'docker rm -f mywebapp || true'
            
            // Run the Docker container    
            sh 'docker run -d -p 4040:8080 --name mywebapp ajay/myapp.0.1'
        }    
}
