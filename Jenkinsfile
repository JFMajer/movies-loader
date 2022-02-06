def imageName = 'jfmajer/movies-loader'
def registry = 'https://public.ecr.aws/d0t2w1c9/jfmajer/movies-loader'

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
    stage('grab credentials') {
        sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/d0t2w1c9'
    }
    stage('Push') {
        docker.withRegistry(registry, 'registry') {
            docker.image(imageName).push(env.BUILD_ID)
        }
    }
}
