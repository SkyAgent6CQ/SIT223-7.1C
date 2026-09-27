pipeline {
    agent any

    // Polls GitHub every ~2 minutes; a build starts only if there is a new commit.
    // NOTE: the trigger is registered after the FIRST manual "Build Now".
    triggers {
        pollSCM('H/2 * * * *')
    }

    environment {
        STAGING_ENV    = 'AWS EC2 staging instance'
        PRODUCTION_ENV = 'AWS EC2 production instance'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Task: Compile the source code and package it into a deployable artefact (e.g. a .jar).'
                echo 'Tool: Maven (build automation tool)'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo 'Task: Run unit tests to check each function works as expected, then integration tests to check components work together.'
                echo 'Tools: JUnit (unit tests), Mockito (mocking dependencies), TestNG (integration tests)'
            }
        }
        stage('Code Analysis') {
            steps {
                echo 'Task: Statically analyse the code for bugs, code smells and duplication to ensure it meets industry standards.'
                echo 'Tool: SonarQube (integrated via the SonarQube Scanner for Jenkins plugin)'
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Task: Scan the code and its dependencies for known vulnerabilities (CVEs).'
                echo 'Tools: OWASP Dependency-Check, Snyk'
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo "Task: Deploy the packaged application to the staging server: ${env.STAGING_ENV}"
                echo 'Tool: AWS CodeDeploy (alternatively Ansible over SSH)'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Task: Run integration/end-to-end tests against staging to confirm the app works in a production-like environment.'
                echo 'Tools: Selenium (UI tests), Postman/Newman (API tests)'
            }
        }
        stage('Deploy to Production') {
            steps {
                echo "Task: Deploy the verified application to the production server: ${env.PRODUCTION_ENV}"
                echo 'Tool: AWS CodeDeploy'
            }
        }
    }

    post {
        success { echo 'Pipeline completed successfully.' }
        failure { echo 'Pipeline failed - check the stage logs above.' }
    }
}
