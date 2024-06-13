pipeline {
    agent any
    stages {
        stage('helm chart validations') {
            parallel {
                stage('helm lint') {
                    steps {
                        sh '''
                            helm lint . --strict
                        '''
                    }
                }

                stage('helm template') {
                    steps {
                        sh '''
                            helm template .
                        '''
                    }
                }
            }
        }
        stage('Release') {
            tools {
                nodejs "nodejs"
            }
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github-app', passwordVariable: 'GITHUB_TOKEN', usernameVariable: 'GITHUB_USERNAME')]) {
                        sh '''
                            npx semantic-release
                        '''
                    }
                }
            }
        }
    }
}
