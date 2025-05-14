pipeline {
	agent any
	environment {
		NEW_VERSION = '1.0.0'
	}
	stages {
		stage("build") {
			steps {
				echo 'building the applicaiton...'
				echo "building version ${env.GIT_BRANCH }"
			}
		}
		stage("test") {
			steps {
				echo 'testing the applicaiton...'
			}
		}
		stage("deploy") {
			steps {
				echo 'deploying the applicaiton...'
			}
		}
	}
}