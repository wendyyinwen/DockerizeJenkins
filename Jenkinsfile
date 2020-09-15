node {
    
	

    env.AWS_ECR_LOGIN=true
    def newApp
    def registry = 'yinwenkueh/dev_op'
    def registryCredential = 'dockerhub'
	tools {nodejs "node" }
	stage('Git') {
		git 'https://github.com/wendyyinwen/DockerizeJenkins .git'
	}
	stage('Build') {
		sh 'npm install'
		sh 'npm run bowerInstall'
	}
	stage('Test') {
		sh 'npm test'
	}
	stage('Building image') {
        docker.withRegistry( 'https://' + registry, registryCredential ) {
		    def buildName = registry + ":$BUILD_NUMBER"
			newApp = docker.build buildName
			newApp.push()
        }
	}
	stage('Registring image') {
        docker.withRegistry( 'https://' + registry, registryCredential ) {
    		newApp.push 'latest2'
        }
	}
    stage('Removing image') {
        sh "docker rmi $registry:$BUILD_NUMBER"
        sh "docker rmi $registry:latest"
    }
    
}
