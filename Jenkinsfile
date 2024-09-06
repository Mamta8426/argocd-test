pipeline {
    agent any

    environment {
        REGISTRY_NAME = 'us-central1-docker.pkg.dev/qa-nextgen/git-repo' // Your GCP Artifact Registry URL
        IMAGE_NAME = 'helloworld'
        IMAGE_TAG = "${env.BUILD_ID}"
    }

    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }

        stage('Update GIT') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([usernamePassword(credentialsId: 'gittoken', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                            sh "git config user.email 'mamta8426@gmail.com'"
                            sh "git config user.name 'Mamta8426'"
                            sh "sed -i 's|test-image2:latest|${REGISTRY_NAME}/${IMAGE_NAME}:${IMAGE_TAG}|g' deployment.yaml"
                            sh "git add ."
                            sh "git commit -m 'Updated Docker image to ${IMAGE_TAG} by Jenkins Job changemanifest: ${env.BUILD_ID}'"
                            sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/Mamta8426/argocd-test HEAD:main"
                        }
                    }
                }
            }
        }
    }
}
