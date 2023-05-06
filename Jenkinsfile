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
        // def dockerRun = "docker run --rm -v ${workspace}:/src cdrx/pyinstaller-linux:python2"

        // sh "${dockerRun} 'pyinstaller --onefile sources/add2vals.py'"

        // archiveArtifacts "dist/add2vals"

        sh 'git config user.email "muh.nur.abdurrauf@gmail.com"'
        sh 'git config user.name "raassh23"'
        sh 'echo $(date +%Y-%m-%d %H-%M-%S) > deploy-timestamp'
        sh 'git add .'
        sh 'git commit -m "Deploy built artifact"'
        sh 'git branch | grep "tmp" && git branch -D tmp'
        sh 'git branch tmp'
        sh 'git checkout master'
        sh 'git merge tmp'
        sh 'git push -f git@heroku.com:python-cicd-pipeline-raassh-23.git master'


        // sleep(time: 1, unit: 'MINUTES')

        // echo 'Deleting app'
        // sh "${dockerRun} 'rm -rf build dist add2vals.spec'"
    }
}

