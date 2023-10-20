pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Publish') {
            when {
                anyOf {
                    branch 'master'
                    branch 'develop'
                }
            }
            steps {
                script {
                    def dockerImage = 'ubuntu-apache-builder'

                    sh "docker run -d --name website-builder -v \$(pwd):/var/www/html $dockerImage /bin/bash -c 'cp -r * /var/www/html'"

                    if (env.BRANCH_NAME == 'master') {
                        sh "docker exec website-builder service apache2 start"
                    }
                }
            }
        }
    }

    post {
        success {
            // Cleanup
            script {
                sh 'docker stop website-builder && docker rm website-builder'
            }
        }
    }
}
