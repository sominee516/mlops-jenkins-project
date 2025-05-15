pipeline {
	agent any
	parameters {
		choice(name: 'VERSION', choices: ['1.1.0','1.2.0','1.3.0'], description: '')
		booleanParam(name: 'executeTests', defaultValue: true, description: '')
	}
	stages {
		stage("init") {
			steps {
				script {
					gv = load "script.groovy"
				}
			}
		}
		stage("Checkout") {
			steps {
				checkout scm
			}
		}
		stage("Build") {
			steps {
				sh 'docker compose build web'
			}
		}
		stage("test") {
			when {
				expression {
					params.executeTests
				}
			}
			steps {
				script {
					gv.testApp()
				}
			}
		}
		stage("Tag and Push") {
			steps {
				withCredentials([[$class: 'UsernamePasswordMultiBinding',
				credentialsId: 'docker-hub', 
				usernameVariable: 'DOCKER_USER_ID', 
				passwordVariable: 'DOCKER_USER_PASSWORD'
				]]) {
					// 태그 붙이기. 태그를 붙일 때는 jenkins-pipeline_web:latest 태그를 붙여준다.
					sh "docker tag jenkins-pipeline_web:latest ${DOCKER_USER_ID}/jenkins-app:${BUILD_NUMBER}"
					
					// 로그인
					sh "docker login -u ${DOCKER_USER_ID} -p ${DOCKER_USER_PASSWORD}"
					
					// Push
					sh "docker push ${DOCKER_USER_ID}/jenkins-app:${BUILD_NUMBER}"
				}
			}
		}
		stage("deploy") {
			steps {
				sh "docker compose up -d"
			}
		}
	}
}
