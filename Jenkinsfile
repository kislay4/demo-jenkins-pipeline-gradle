pipeline {
    agent none

    stages {
        stage('Build') {
            agent { label 'gradle' }
            steps {
                checkout scm
                sh "env"
                sh "gradle bootRepackage --stacktrace"
                sh 'jarFile=`ls build/libs | grep -v original` && mkdir -p ocp/deployments && cp build/libs/$jarFile ocp/deployments/'
            }
        }
        stage('Test') {
            agent { label 'gradle' }
            steps {
                parallel(
                    'check': {
                        sh "gradle check --stacktrace"
                    },
                    'echo': {
                        echo "ok in parallel"
                    }
                )
            }
        }
        
        
    }
}
