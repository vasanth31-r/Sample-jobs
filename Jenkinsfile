pipeline {
    agent any  // Runs on any available Jenkins agent

    environment {
        GIT_CREDENTIALS_ID = 'github-token' // ID of Jenkins GitHub credentials
        REPO_URL = 'https://github.com/vasanth31-r/Sample-jobs'
        BRANCH = 'main' // Change to your working branch
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: "${BRANCH}",
                    credentialsId: "${GIT_CREDENTIALS_ID}",
                    url: "${REPO_URL}"
            }
        }

        stage('Build') {
            steps {
                script {
                    def isMavenProject = fileExists('pom.xml')
                    def isNodeProject = fileExists('package.json')

                    if (isMavenProject) {
                        echo 'Detected Maven project... Running Maven build'
                        sh 'mvn clean install'
                    } else if (isNodeProject) {
                        echo 'Detected Node.js project... Running npm install & build'
                        sh 'npm install'
                        sh 'npm run build'
                    } else {
                        error 'No recognized build file found!'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    def isMavenProject = fileExists('pom.xml')
                    def isNodeProject = fileExists('package.json')

                    if (isMavenProject) {
                        echo 'Running Maven tests'
                        sh 'mvn test'
                    } else if (isNodeProject) {
                        echo 'Running npm tests'
                        sh 'npm test'
                    } else {
                        echo 'No test script found, skipping tests...'
                    }
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar, **/build/*', fingerprint: true
            }
        }

        stage('Deploy (Optional)') {
            steps {
                echo 'Deploying application...'
                // Example: SCP deployment
                // sh 'scp -i /path/to/key target/myapp.jar user@server:/path/to/deploy'
                // Example: Docker deployment
                // sh 'docker-compose up -d'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
