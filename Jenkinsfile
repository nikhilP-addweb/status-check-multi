pipeline {
    agent any
    
    stages {
        stage('Validation') {
            steps {
                script {
                    sh 'composer validate'
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

void setBuildStatus(String message, String state) {
    script {
        step([
            $class: "GitHubCommitStatusSetter",
            reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/nikhilP-addweb/status-check-multi"],
            contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
            errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
            statusResultSource: [$class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]]]
        ])
    }
}
	
