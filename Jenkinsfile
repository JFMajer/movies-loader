def imageName = 'jfmajer/movies-loader'
def registry = 'https://578997275585.dkr.ecr.eu-north-1.amazonaws.com/jfmajer/movies-loader'
def commitID() {
    sh 'git rev-parse HEAD > .git/commitID'
    def commitID = readFile('.git/commitID').trim()
    sh 'rm .git/commitID'
    commitID
}

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
            docker.image(imageName).push(commitID)
        }
    }
}
