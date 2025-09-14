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

// The Final Travel Memory Jenkinsfile
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
                // Run npm install inside the 'backend' folder
                dir('backend') {
                    sh 'npm install'
                }
            }
        }

        stage('Run Tests') {
            // A conditional stage based on our parameter
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
                    // Use credentials securely
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'swati-aws-creds']])
                        echo "Logging into AWS..."
                        // sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        sh "aws s3 ls"

                        // Create a unique tag for our image using the Jenkins build number
                        // def taggedImage = "${env.DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}"
                        echo "${env.app_name} running in aws"
                        // echo "Building image: ${taggedImage}"
                        // Build the Docker image from the Dockerfile in the current directory
                        // sh "docker build -t ${taggedImage} ."
                                              
                        // echo "Pushing image: ${taggedImage}"
                        // Push the newly built image to Docker Hub
                        // sh "docker push ${taggedImage}"
                    }
                }
            }
        }
    }
    
    // Post-build actions for cleanup
    post {
        always {
            echo "Pipeline finished. Cleaning up."
            // Always logout to be safe
            // sh 'docker logout'
        }
        success {
            echo "Image pushed successfully!"
        }
    }



