pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-token',
                    url: 'https://github.com/vasanth31-r/Sample-jobs.git'
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
                        echo '⚠️ No build file found, skipping build step...'
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
                        echo '⚠️ No test file found, skipping test step...'
                    }
                }
            }
        }
    }
}
