node {
	withDockerContainer('python:2-alpine') {
		stage('Build') {
			checkout scm
			sh 'python -m py_compile sources/add2vals.py sources/calc.py'
		}
        }
	withDockerContainer('qnib/pytest') {
		stage('Test') {
			checkout scm
			// sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
			sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
			junit 'test-reports/results.xml'
		}
	}
	stage('Manual Approval') {
		checkout scm
		input message: 'Lanjutkan ke tahap Deploy?'
	}
	stage('Deploy') {
		checkout scm
		sh 'docker run --rm -v /var/jenkins_home/workspace/simple-python-pyinstaller-app/sources:/src cdrx/pyinstaller-linux:python2 \'pyinstaller -F add2vals.py\''
		archiveArtifacts artifacts: 'sources/add2vals.py', followSymlinks: false
		sh 'docker run --rm -v /var/jenkins_home/workspace/simple-python-pyinstaller-app/sources:/src cdrx/pyinstaller-linux:python2 \'rm -rf build dist\''
		sleep time: 1, unit: 'MINUTES'
	}
}
