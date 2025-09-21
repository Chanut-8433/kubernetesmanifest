node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh "git config user.email linuxgraytraining@gmail.com"
                    sh "git config user.name linuxgray"
                    
                    // Print before update
                    sh "cat deployment.yaml"
                    
                    sh "sed -i 's+linuxgray039/test.*+linuxgray039/test:${DOCKERTAG}+g' deployment.yaml"

                    
                    // Print after update
                    sh "cat deployment.yaml"
                    
                    // Git commit and push
                    sh "git add ."
                    sh "git commit -m 'Updated image tag to latest by Jenkins Job: ${env.BUILD_NUMBER}'"
                    sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
                }
            }
        }
    }
}
