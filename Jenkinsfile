node {
  stage('Clone Repo') {
    checkout scm
  }

  stage('Update Manifest File') {
    script {
      catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
        withCredentials([usernamePassword(
          credentialsId: 'github',
          usernameVariable: 'GIT_USERNAME',
          passwordVariable: 'GIT_PASSWORD'
        )]) {
          sh '''
            git config user.email "abhishekrangra@gmail.com"
            git config user.name "AbhishekRangra"

            echo "k8s-app.yaml before the update:"
            cat k8s-app.yaml

            sed -i 's+abhishekrangra/pyabhi.*+abhishekrangra/pyabhi:'"$DOCKERTAG"'+g' k8s-app.yaml

            echo "k8s-app.yaml after the update:"
            cat k8s-app.yaml

            git add k8s-app.yaml
            git commit -m "Job updated image tag to $BUILD_NUMBER"
            git push https://$GIT_USERNAME:$GIT_PASSWORD@github.com/$GIT_USERNAME/pythonapp-k8s.git HEAD:main
          '''
        } // close withCredentials
      } // close catchError
    } // close script
  } // close stage
} // close node
