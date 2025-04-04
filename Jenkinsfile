pipeline {
    agent any

    def clonePythonGreetingsRepo() {
        git 'https://github.com/mtararujs/python-greetings'
    }

    def cloneCourseJsApiFrameworkRepo() {
        git 'https://github.com/mtararujs/course-js-api-framework'
    }

    def deployToEnvironment(environment, port) {
        echo "Deploying to $environment environment..."
        clonePythonGreetingsRepo()
        sh "pm2 delete greetings-app-$environment || true"
        sh "pm2 start app.py --name greetings-app-$environment --port $port"
    }

    def runTestsOnEnvironment(environment) {
        echo "Running tests on $environment environment..."
        cloneCourseJsApiFrameworkRepo()
        sh 'npm install'
        sh "npm run greetings greetings_$environment"
    }

    stages {
        stage('install-pip-deps') {
            steps {
                echo "Installing pip dependencies..."
                clonePythonGreetingsRepo()
                sh 'ls'
                sh 'pip install -r requirements.txt'
            }
        }
        stage('deploy-to-dev') {
            steps {
                deployToEnvironment('dev', 7001)
            }
        }
        stage('tests-on-dev') {
            steps {
                runTestsOnEnvironment('dev')
            }
        }
        stage('deploy-to-staging') {
            steps {
                deployToEnvironment('staging', 7002)
            }
        }
        stage('tests-on-staging') {
            steps {
                runTestsOnEnvironment('staging')
            }
        }
        stage('deploy-to-preprod') {
            steps {
                deployToEnvironment('preprod', 7003)
            }
        }
        stage('tests-on-preprod') {
            steps {
                runTestsOnEnvironment('preprod')
            }
        }
        stage('deploy-to-prod') {
            steps {
                deployToEnvironment('prod', 7004)
            }
        }
        stage('tests-on-prod') {
            steps {
                runTestsOnEnvironment('prod')
            }
        }
    }
}
