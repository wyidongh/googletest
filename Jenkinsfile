pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yourname/cpp-ci-lab.git'
            }
        }

        stage('Build') {
            steps {
                sh '''
                mkdir -p build
                cd build
                cmake ..
                make
                '''
            }
        }

        stage('Unit Test') {
            steps {
                sh '''
                cd build
                ctest --output-on-failure
                '''
            }
        }

        stage('Generate Test Report') {
            steps {
                sh '''
                cd build
                ctest -T Test
                '''
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'build/**', fingerprint: true
            }
        }

        stage('Deploy (Mock)') {
            steps {
                sh '''
                echo "Deploying to target machine..."
                mkdir -p /tmp/deploy
                cp build/* /tmp/deploy || true
                '''
            }
        }
    }

    post {
        success {
            mail to: 'your_email@example.com',
                 subject: "CI Success: ${env.JOB_NAME}",
                 body: "Build成功 ✅\nJob: ${env.JOB_NAME}\nBuild: ${env.BUILD_NUMBER}"
        }

        failure {
            mail to: 'your_email@example.com',
                 subject: "CI Failed: ${env.JOB_NAME}",
                 body: "Build失败 ❌ 请检查日志"
        }
    }
}
