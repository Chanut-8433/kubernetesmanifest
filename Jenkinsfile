node {
    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh """
                        git config user.email "chanut.non@gmail.com"
                        git config user.name "Chanut-8433"

                        echo "Before update:"
                        grep 'image:' deployment.yaml || true

                        # Update only the image line
                        sed -i "s|image: chanut8433/test.*|image: chanut8433/test:${DOCKERTAG}|g" deployment.yaml

                        echo "After update:"
                        grep 'image:' deployment.yaml || true

                        git add deployment.yaml
                        git commit -m "Update image tag to ${DOCKERTAG} by Jenkins Build ${BUILD_NUMBER}" || echo "No changes to commit"

                        set +x
                        git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/Chanut-8433/kubernetesmanifest.git HEAD:main
                        set -x
                    """
                }
            }
        }
    }
}
