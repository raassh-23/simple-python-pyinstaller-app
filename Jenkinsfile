node {
    def workspace = pwd()
    echo "Workspace: ${workspace}"
    stage('Build') {
        docker.image('python:2-alpine').inside("-v ${workspace}:/app") {
            sh 'cd /app && pwd && ls -la'
            sh 'cd /app && python -m py_compile app/sources/add2vals.py sources/calc.py'
        }
    }
    stage('Test') {
        docker.image('qnib/pytest').inside("-v ${workspace}:/app") {
            sh 'cd /app && py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        post {
            always {
                junit 'test-reports/results.xml'
            }
        }
    }
}