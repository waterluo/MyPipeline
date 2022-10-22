pipeline {
    agent any
    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE    = 'sqlite'
    }
    stages {
        stage('No-op') {
            steps {
                sh 'ls'
            }
        }
        stage('Build') {
            steps {
                sh 'printenv'
                sh 'python3 --version'
                sh 'echo "Hello World"'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Fail!"; exit 1'
                // sh 'echo "Test success!"'
            }
        }
        stage('Deploy') {
            steps {
                retry(3) {
                    sh 'echo "flakey-deploy"'
                }
                timeout(time: 3, unit: 'MINUTES') {
                    sh 'echo "health-check"'
                }
            }
        }
    }
    post {
        always {
            echo 'always run'
        }
        success {
            echo 'run only if successful'
        }
        failure {
            echo 'run only if failed'
            mail to: '773738584@qq.com', subject: "Failed Pipeline: ${currentBuild.fullDisplayName}", body: "Something is wrong with ${env.BUILD_URL}"
        }
        unstable {
            echo 'run only if the run was marked as unstable'
        }
        changed {
            echo 'run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}