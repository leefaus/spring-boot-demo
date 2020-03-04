pipeline {
    environment {
        registry = "leefaus/spring-boot-demo"
        registryCredential = "dockerhub"
        dockerImage = ""
    }
    agent any
    tools {
        gradle 'gradle621'
        jdk 'jdk8'
    }

    stages {
        stage('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                '''
            }
        }
        stage('Unit & Integration Tests') {
            steps {
                script {
                    try {
                        sh './gradlew clean test --no-daemon' //run a gradle task
                    } finally {
                        junit '**/build/test-results/test/*.xml' //make the junit test results available in any case (success & failure)
                    }
                }
            }
        }
        stage('Create JAR') {
            steps {
                script {
                    sh './gradlew clean bootJar --no-daemon' //run a gradle task
                }
            }
        }
        stage ('Docker Build') {
            steps {
                script {
                    dockerImage = docker.build (registry + ":${BUILD_NUMBER}", "--build-arg JARFILE=spring-boot-demo-0.0.1-SNAPSHOT.jar .")
                }
            }
        }
        stage ('Docker Publish') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}