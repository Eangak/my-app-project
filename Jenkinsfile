Jenkins for git tag: 

pipeline {
    agent any
    
    environment {
        // Registry & Container Configuration
        GITHUB_REPOSITORY="https://github.com/Eangak/my-app-project.git"
    }
    
    parameters {
        gitParameter(name: 'TAG', type: 'PT_TAG', defaultValue: '', description: 'Select the Git tag to build.')
        gitParameter(name: 'BRANCH', type: 'PT_BRANCH', defaultValue: '', description: 'Select the Git branch to build.')
          // Parameter for selecting the deployment action
        choice(name: 'ACTION',choices: ['deploy', 'rollback'],description: 'Choose whether to deploy a new version or rollback to a previous version.')
    }
    post {
        always {
            echo "Cleaning up workspace..."
            cleanWs()
        }
    }
    stages {
        stage('Checkout Code') {
            steps {
                script {
                    echo "Action: ${params.ACTION} | Branch: ${params.BRANCH} | Revision: ${params.VERSION_COMMIT}"
                     if (params.TAG) {
                        echo "Checking out tag: ${params.TAG}"
                        checkout([$class: 'GitSCM',
                            branches: [[name: "refs/tags/${params.TAG}"]],
                            userRemoteConfigs: [[url: env.GITHUB_REPOSITORY]]
                        ])
                      } else {
                          echo "Checking out branch: ${params.BRANCH}"
                          checkout([$class: 'GitSCM',
                              branches: [[name: "${params.BRANCH}"]],
                              userRemoteConfigs: [[url: env.GITHUB_REPOSITORY]]
                          ])
                      }
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    echo "Building the application..."
                    // Add your build commands here
                }
            }
        }
        stage('push') {
            steps {
                script {
                    echo "Pushing the application..."
                    // Add your push commands here
                }
            }
        }
        stage('Deploy') {
            when {
                expression { params.ACTION == 'deploy' }
            }
            steps {
                script {
                    echo "Deploying the application..."
                    // Add your deployment commands here
                }
            }
        }
        stage('Rollback') {
            when {
                expression { params.ACTION == 'rollback' }
            }
            steps {
                script {
                    echo "Rolling back the application..."
                    // Add your rollback commands here
                }
            }
        }
    }
}