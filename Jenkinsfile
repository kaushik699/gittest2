pipeline{
    agent any
    stages {
        stage('checkout'){
            steps{
                echo 'checking out code'
            }
        }
        stage('Install dependencies'){
            steps{
                echo 'Installing dependencies'
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install -r requirements.txt
                '''
            }
        }
        stage('Run Tests') {
      steps {
        echo '✅ Running unit tests...'
        sh '''
          . venv/bin/activate
          pytest --junitxml=test-results.xml
        '''
      }
      post {
        always {
          junit 'test-results.xml'
        }
      }
    }
     stage('Archive Artifacts') {
      steps {
        echo '📦 Archiving build output...'
        sh 'mkdir -p output && echo "Build result file" > output/dummy.txt'
        archiveArtifacts artifacts: 'output/**', fingerprint: true
      }
    }
    }
    post {
    success {
      echo '✅ Pipeline succeeded!'
    }
    failure {
      echo '❌ Pipeline failed!'
    }
  }
}