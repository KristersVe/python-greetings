pipeline {
    agent any
    triggers { 
        pollSCM("*/1 * * * *") 
    }

    options {
        disableConcurrentBuilds()
    }

    stages {
        stage("Build docker image") {
            steps {
                print_text("Building docker image")
                git branch: 'main', url: 'https://github.com/KristersVe/python-greetings.git'
                sh "docker build -t kristersv/python-greetings-app:latest ."
                sh "docker push kristersv/python-greetings-app:latest"
            }
        }
        
        stage("Deploy to development") {
            steps {
                print_text("Deploying to development")
                deploy_on("greetings-app-dev")
            }
        }
        
        stage("Test on development") {
            steps {
                print_text("Testing on development")
                test_on("dev")
            }
        }
        
        stage("Deploy to staging") {
            steps {
                print_text("Deploying to staging")
                deploy_on("greetings-app-stg")
            }
        }
        
        stage("Test on staging") {
            steps {
                print_text("Testing on staging")
                test_on("stg")
            }
        }
        
        stage("Deploy to production") {
            steps {
                print_text("Deploying to production")
                deploy_on("greetings-app-prod")
            }
        }
        
        stage("Test on production") {
            steps {
                print_text("Testing on production")
                test_on("prod")
            }
        }
    }
}


def print_text(text){
    echo text
}

def deploy_on(env) {
    sh "docker pull kristersv/python-greetings-app:latest"
    sh "docker-compose -f docker-compose.yml stop ${env}"
    sh "docker-compose -f docker-compose.yml rm -f ${env}"
    sh "docker-compose -f docker-compose.yml up -d ${env}"
}

def test_on(env) {
    sh "docker pull kristersv/api-tests:latest"
    sh "docker run --network=host --rm kristersv/api-tests:latest run tests/scenarios/greetings greetings_${env}"
}
