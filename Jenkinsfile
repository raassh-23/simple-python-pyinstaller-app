node {
    def workspace = pwd()

    stage('Build') {
        docker.image('python:2-alpine').inside("-v ${workspace}:/app") {
            sh 'cd /app && python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }

    stage('Test') {
        try {
            docker.image('qnib/pytest').inside("-v ${workspace}:/app") {
                sh 'cd /app && py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
        } finally {
            junit '/app/test-reports/results.xml'
        }
    }
}