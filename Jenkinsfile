def imageName = 'jfmajer/movies-loader'
def registry = 'https://578997275585.dkr.ecr.eu-north-1.amazonaws.com/jfmajer/movies-loader'

node ('workers') {
    stage('Checkout') {
        checkout scm
    }
    stage('Unit Tests') {
        def imageTest = docker.build("${imageName}-test","-f Dockerfile.test .")
    }
    stage('Build') {
        docker.build(imageName)
    }
    stage('Push') {
        docker.withRegistry(registry, 'ecr:eu-north-1:my.aws.credentials') {
            docker.image(imageName).push(env.BUILD_ID)
        }
    }
}
