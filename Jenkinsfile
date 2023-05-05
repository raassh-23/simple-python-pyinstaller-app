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
        sh "docker run --rm -v ${workspace}:/src cdrx/pyinstaller-linux:python2 'pyinstaller --onefile sources/add2vals.py'"

        archiveArtifacts "dist/add2vals"

        // sleep(time: 1, unit: 'MINUTES')

        echo 'Deleting app'
        sh "docker run --rm -v ${workspace}:/src cdrx/pyinstaller-linux:python2 'rm -rf build dist add2vals.spec'"

        sh 'ls'
    }
}