pipeline {
    agent any

    environment {
        REGISTRY_NAME = 'europe-west2-docker.pkg.dev/onecom-operations/git-repo' // Your GCP Artifact Registry URL
        IMAGE_NAME = 'test-image2'
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
                        withCredentials([usernamePassword(credentialsId: 'github_token', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                            sh "git config user.email 'mamta.guliya@onecom.co.uk'"
                            sh "git config user.name 'MamtaGuliya'"
                            sh "sed -i 's|test-image2:latest|${REGISTRY_NAME}/${IMAGE_NAME}:${IMAGE_TAG}|g' deployment.yaml"
                            sh "git add ."
                            sh "git commit -m 'Updated Docker image to ${IMAGE_TAG} by Jenkins Job changemanifest: ${env.BUILD_ID}'"
                            sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/MamtaGuliya/k8s-test.git HEAD:main"
                        }
                    }
                }
            }
        }
    }
}
