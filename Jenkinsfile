node {
  checkout scm
  docker.image('python:3.12.1-alpine3.19').inside('-p 3000:3000') {
    withEnv([CI = true]) {
      stage('Build') {
        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        stash name: 'compiled-results', includes: 'sources/*.py*'
      }
      
      stage('Test') {
        try {
          docker.image('qnib/pytest').inside('-p 3000:3000') {
            sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
          }
        } catch (e) {
          echo "Test Stage Failed!"
        } finally {
          junit 'test-reports/results.xml'
        }
      }
    }
  }
}
