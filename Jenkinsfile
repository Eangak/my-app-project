Jenkins for git tag: 

pipeline {
    agent any
    
    environment {
        // Registry & Container Configuration
        GITHUB_REPOSITORY="https://github.com/krolnoeurn36/usea-first-app.git"
    }
    
    parameters {
        gitParameter(name: 'TAG', type: 'PT_TAG', defaultValue: '', description: 'Select the Git tag to build.')
        gitParameter(name: 'BRANCH', type: 'PT_BRANCH', defaultValue: '', description: 'Select the Git branch to build.')
          // Parameter for selecting the deployment action
        choice(name: 'ACTION',choices: ['deploy', 'rollback'],description: 'Choose whether to deploy a new version or rollback to a previous version.')
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
    }
}