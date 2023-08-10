pipeline {
    agent any
    
    stages {
        stage('Validation') {
            steps {
                script {
                    sh 'cd /var/lib/jenkins/jobs/status-check-decl-pipe/workspace'
                    sh 'composer install'
                }
            }
        }
    }
    
    post {
        success {
            script {
                setBuildStatus("Validation succeeded", "SUCCESS")
            }
        }

        failure {
            script {
                setBuildStatus("Validation failed", "FAILURE")
            }
        } 
    }
}

def setBuildStatus(String message, String state) {
    script {
        withCredentials([string(credentialsId: 'b0d87f19-c0a4-42d1-97cf-090bf400d5a5', variable: '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')]) {
            step([
                $class: "GitHubCommitStatusSetter",
                reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/nikhilP-addweb/status-check-multi"],
                contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
                errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
                statusResultSource: [$class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]]],
                authentication: [$class: "com.cloudbees.plugins.credentials.common.UsernamePasswordCredentialsImpl", credentialsId: "b0d87f19-c0a4-42d1-97cf-090bf400d5a5"]
            ])
        }
    }
}

