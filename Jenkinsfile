node() {
  stage('Build') {
    docker.image('python:3.12.1-alpine3.19').inside('-p 3000:3000') {
      checkout scm
      sh 'python -m py_compile sources/add2vals.py sources/calc.py'
      stash(name: 'compiled-results', includes: 'sources/*.py*')
    }
  }

  stage('Test') {
    docker.image('qnib/pytest').inside('-p 3000:3000') {
      checkout scm
      sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
      junit 'test-reports/results.xml'
    }
  }
}