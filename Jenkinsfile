node {
  stage('Clone repository') {
    checkout scm
  }

  stage('Update GIT') {
    script {
      catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
        withCredentials([
          usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_TOKEN')
        ]) {
          sh """
            set -euxo pipefail
            git config user.email 'chanut.non@gmail.com'
            git config user.name  'chanut-8433'

            echo '--- BEFORE ---'
            sed -n '1,120p' deployment.yaml

            # 1) อัปเดต image tag ให้เป็นพารามิเตอร์ DOCKERTAG
            sed -i -E 's#^(\\s*image:\\s*)chanut8433/test-2:\\S+#\\1chanut8433/test-2:${DOCKERTAG}#' deployment.yaml

            echo '--- AFTER ---'
            sed -n '1,120p' deployment.yaml

            # 2) สร้าง/รีเซ็ต main ให้อยู่บน branch ไม่ใช่ detached
            git checkout -B main origin/main || git checkout -B main

            # 3) commit เฉพาะตอนมี diff
            git add deployment.yaml
            if git diff --cached --quiet; then
              echo 'No changes to commit'
            else
              git commit -m 'Update image to chanut8433/test-2:${DOCKERTAG} (by Jenkins ${BUILD_NUMBER})'
            fi

            # 4) push กลับ repo ของคุณ
            git push https://${GIT_USER}:${GIT_TOKEN}@github.com/Chanut-8433/kubernetesmanifest.git main
          """
        }
      }
    }
  }
}
