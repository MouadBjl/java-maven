
pipeline {
    agent {
        docker {
            image 'my-maven-git:latest'
            args '-v maven-repo:/root/.m2'
        }
    }

    options {
        skipDefaultCheckout(true)
    }

    stages {

        stage('Checkout') {
            steps {
                deleteDir()

                git branch: 'main',
                    url: 'https://github.com/MouadBjl/java-maven.git'
            }
        }

        stage('Debug Git') {
            steps {
                sh 'pwd'
                sh 'ls -la'
                sh 'git status'
            }
        }

        stage('Build & Test') {
            steps {
                dir('maven') {
                    sh 'mvn -B clean verify'

                    archiveArtifacts(
                        artifacts: 'target/*.jar',
                        fingerprint: true
                    )
                }
            }
        }

        stage('Run') {
            steps {
                dir('maven') {
                    sh 'java -jar target/maven-1.1.1-SNAPSHOT.jar'
                }
            }
        }
    }

    post {
        success {
            echo 'BUILD SUCCESSFUL!'
        }

        failure {
            echo 'BUILD FAILED!'
        }
    }
}
