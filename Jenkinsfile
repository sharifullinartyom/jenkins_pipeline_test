pipeline {
    agent none

    environment {
        NEXUS      = '45.155.205.81:8124'
        APP        = 'my-java-app'
        IMAGE      = "${NEXUS}/${APP}"
        TAG        = "${env.TAG}"
        CONTAINER  = 'my-java-app'
    }

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    stages {        
        stage('Build project') {
            agent { label 'build' }
            steps {
                git(
                    url: 'https://github.com/sharifullinartyom/jenkins_pipeline_test.git',
                    branch: 'main'
                )       
                script {
                    def img = docker.build("${IMAGE}:${TAG}")

                    docker.withRegistry("http://${NEXUS}", 'nexus-docker-creds') {
                        img.push()
                        img.push('latest')
                    }
                }
            }
        }
    }
}
