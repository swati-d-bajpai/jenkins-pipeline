// pipeline {
//     agent { node 'swati-node' }
//     stages {
//         stage('Checkout') {
//             steps {
//                 git branch: 'main', url: 'https://github.com/swati-d-bajpai/jenkins-pipeline.git'
//             }
//         }
//         stage('Install') {
//             steps {
//                 sh 'cd backend; npm install'
//             }
//         }
//         stage('Build') {
//             steps {
//                 sh 'cd backend; npm run build'
//             }
//         }
//     }

// }

pipeline {
    // 1. Use the agent we configured
    agent { node 'swati-node' }

    // 2. Define the non-secret variables
    environment {
        DOCKER_IMAGE_NAME = 'your-dockerhub-username/travel-memory' // CHANGE THIS
        app_name = 'swati_travel_memory'
    }
    
    // 3. Define the parameters for flexibility (for manual runs)
    parameters {
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Check this to run unit tests.')
    }

    // 4. Define the automatic trigger for this pipeline
    triggers {
        // This enables the pipeline to be started automatically by a GitHub webhook on a 'git push'
        githubPush()
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/swati-d-bajpai/jenkins-pipeline.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Run Tests') {
            when { expression { return params.RUN_TESTS } }
            steps {
                dir('backend') {
                    sh 'npm run test'
                }
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                dir('backend') {
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'swati-aws-creds']]) {
                        echo "Logging into AWS..."
                        sh "aws s3 ls"
                        echo "${env.app_name} running in aws"

                        // Example for Docker build & push (uncomment if needed)
                        // def taggedImage = "${env.DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}"
                        // echo "Building image: ${taggedImage}"
                        // sh "docker build -t ${taggedImage} ."
                        // echo "Pushing image: ${taggedImage}"
                        // sh "docker push ${taggedImage}"
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished. Cleaning up."
            // sh 'docker logout'
        }
        success {
            echo "Image pushed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs!"
        }
    }
}
