pipeline {
    agent any
	

 stages {
    stage('checkout') {
           steps {
             
                git branch: 'main', url: 'https://github.com/srikanta1219/devops-practical-2.git'
             
          }
        }
       

    stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t sri-app:latest .' 
                sh 'docker tag sri-app srikanta1219/sri-app:$BUILD_NUMBER'
               
          }
        }
    stage('Publish image to Docker Hub') {
            steps {
        withDockerRegistry([ credentialsId: "DockerHub", url: "" ]) {
           sh  'docker push srikanta1219/sri-app:$BUILD_NUMBER' 
		    }      
          }
        }
    stage('Update deployment file') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        //withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
    
                        withCredentials([usernamePassword(credentialsId: 'GitHub', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                            //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                            sh "git config user.email srikantanayak41@gmail.com"
                            sh "git config user.name srikanta1219"
                            //sh "git switch master"
                            sh "cat deployment.yaml"
                            sh "sed -i 's+srikanta1219/sri-app.*+srikanta1219/sri-app:${DOCKERTAG}+g' deployment.yaml"
                            sh "cat deployment.yaml"
                            sh "git add ."
                            sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                            sh "git push --force https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/devops-practical-2.git HEAD:main"
                            //sh "git push https://github.com/srikanta1219/update-k8s-manifest.git"
                            
                        }
                    }
                }
            }

                  
        }
    }
}    
		
