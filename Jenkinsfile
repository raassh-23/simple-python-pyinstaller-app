node {
    def workspace = pwd()

    stage('Build') {
        docker.image('python:2-alpine').inside("-v ${workspace}:/app") {
            sh 'cd /app && python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }

    stage('Test') {
        docker.image('qnib/pytest').inside("-v ${workspace}:/app") {
            try {
                sh 'cd /app && py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            } catch (Exception e) {
                echo "Exception: ${e}"
            } finally {
                junit 'test-reports/results.xml'
            }
        }
    }

    stage('Manual Approval') {
        input message: 'Lanjutkan ke tahap Deploy?'
    }

    stage('Deploy') {
        docker.image('cdrx/pyinstaller-linux:python2').inside("-v ${workspace}:/app --entrypoint") {
            pip list
            sh 'cd /app && pyinstaller --onefile sources/add2vals.py'
            echo "test1"
            sh 'ls -la app/'
            echo "test2"
            sh 'ls -la dist/'
            echo "test3"
            sh 'ls -la app/dist/'
        }

        // sleep(time: 1, unit: 'MINUTES')
        // echo 'Finished Deployed!'
    }
}