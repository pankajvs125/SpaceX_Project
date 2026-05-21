pipeline{
	agent any
	stages{
		stage("S3 Bucket"){
			steps{
				sh 'aws s3 cp . s3://spacex-project --recursive --exclude ".git/*" '
				}
			}
		stage("docker image"){
			steps{
				sh 'docker build -t spacex .'
				}
			}
		stage("docker image push"){
			steps{
				script{
					withDockerRegistry(credentialsId: 'e75fc66c-b3f7-41df-853d-a5702c993487') {
						sh 'docker tag spacex pankajvs125/spacex:v$BUILD_ID'
						sh 'docker push pankajvs125/spacex:v$BUILD_ID'
					}
				}
			}
		}
		stage("docker container"){
			steps{
				sh 'docker run -d -p 9000:80 spacex'
			}
		}
	}
	post{
		failure{
			emailext(
				subject:'SpaceX Project',
				body:'Project got failed',
				to:'siddardha070@gmail.com,pankajvs125@gmail.com,hasmita1919@gmail.com'
			)
		}
		success{
			emailext(
				subject:'SpaceX Project',
				body:'Project got Sucess IP address 15.206.127.242:9000',
                to:'siddardha070@gmail.com,pankajvs125@gmail.com,hasmita1919@gmail.com'
			)
		}
	}
}
