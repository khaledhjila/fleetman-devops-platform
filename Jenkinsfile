pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
                echo 'Fleetman source code checked out successfully'
            }
        }

        stage('Build Java Microservices') {
            steps {
                script {
                    def services = [
                        'k8s-fleetman-api-gateway',
                        'k8s-fleetman-position-simulator',
                        'k8s-fleetman-position-tracker'
                    ]

                    for (service in services) {
                        dir("microservice-source-code/release2/${service}") {
                            sh '''
                                export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
                                export PATH="$JAVA_HOME/bin:$PATH"
                                chmod +x mvnw
                                ./mvnw -B clean package -DskipTests
                            '''
                        }
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    def services = [
                        'k8s-fleetman-api-gateway',
                        'k8s-fleetman-position-simulator',
                        'k8s-fleetman-position-tracker'
                    ]

                    for (service in services) {
                        dir("microservice-source-code/release2/${service}") {
                            sh '''
                                export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
                                export PATH="$JAVA_HOME/bin:$PATH"
                                ./mvnw -B test
                            '''
                        }
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'SonarQube analysis will be configured after validating the build and tests.'
            }
        }
    }
}
