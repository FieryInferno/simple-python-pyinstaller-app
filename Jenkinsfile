node {
	withDockerContainer('python:2-alpine') {
		stage('Build') {
			checkout scm
			sh 'python -m py_compile sources/add2vals.py sources/calc.py'
		}
        }
	stage('Deploy') {
		checkout scm
		sh 'docker run --rm -v /var/jenkins_home/workspace/simple-python-pyinstaller-app/sources:/src cdrx/pyinstaller-linux:python2 \'pyinstaller -F add2vals.py\''
		archiveArtifacts artifacts: 'sources/add2vals.py', followSymlinks: false
		sh 'docker run --rm -v /var/jenkins_home/workspace/simple-python-pyinstaller-app/sources:/src cdrx/pyinstaller-linux:python2 \'rm -rf build dist\''
		sleep time: 1, unit: 'MINUTES'
	}
}
