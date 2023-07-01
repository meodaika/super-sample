pipeline {
    agent any

    environment {
        // Please update your own registry here
        REGISTRY = 'localhost:8085'
        REGISTRY_IMAGE = "$REGISTRY/private/jenkins-example"
        DOCKERFILE_PATH = 'Dockerfile'

        REGISTRY_USER = credentials('registryUser')
        REGISTRY_PASSWORD = credentials('registryPassword')

        CURRENT_BUILD_NUMBER = "${currentBuild.number}"
        GIT_COMMIT_SHORT = sh(returnStdout: true, script: "git rev-parse --short ${GIT_COMMIT}").trim()
    }

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t $REGISTRY_IMAGE:$GIT_COMMIT_SHORT-jenkins-$CURRENT_BUILD_NUMBER -f $DOCKERFILE_PATH .'
            }
        }
        stage('Push') {
            steps {
                script {
                def command = '''
                docker login -u 'robot$jenjen' -p 'eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE2OTA3ODQ5MjYsImlhdCI6MTY4ODE5MjkyNiwiaXNzIjoiaGFyYm9yLXRva2VuLWRlZmF1bHRJc3N1ZXIiLCJpZCI6MiwicGlkIjozLCJhY2Nlc3MiOlt7IlJlc291cmNlIjoiL3Byb2plY3QvMy9yZXBvc2l0b3J5IiwiQWN0aW9uIjoicHVzaCIsIkVmZmVjdCI6IiJ9XX0.KT0P5vsdL4xF2fZd17lCIDm6o-1wHcf44hx7wltuSv8BIQ1B7iV2WIHKBDXnQF2U-rNznzamT_smNrut373uVh6dFveCJL5aSpwbU0g1buCIITARSv_sdvJH7jT_h7bVFP-jy6_CLwiBsBEoqHAgCnsyVpPs1vJUf_14I3OYnI997isUYk4tV-S-0a175Paj1LvVB7dtxzKu7QR0RhHjOvHOMHeZx_ICufQKWCCYr0mD5poR1p2VqfWaxxJeJyNCsadvQI7mD-9ez08_BmtGflZUXUkGOhk_PSTd9fRXvcAN2xAGi1Xrt_DVC6mKZz0yRXryY70UmxAMKiMigS34f4O3oJnFkDKcjnSI86jpRM2KRGHfSYuBRq4CgdXDfwvtQDVO13wE0uEzY6qJGBldfWVCINwq0-wq4s9tOWSS-IMlqnS92uRrOGNjT7X3Pi9oQvVFkgQihAAsCzF00dEhTpMCjdgBUm7En40BzjlNA-rAIiZ-P2OU7nAUkxKd8f-GNRH_81qDb6ROH4otgnJBGnJEMZy_xRwocPejsocqBId_rgg9hYYpIL75Wa_XHv2UkOxrs6NXgx9BqyXVWh5AiUZUVXkH5nrotgXI0QBL_OIsM2apeu6Fd-9XNOWuzoje-eQ1kMIZewYDxea0sqI39mmnSbYCYUqL4UwRmV7tiJk' $REGISTRY
            '''
                sh command
                sh 'docker push $REGISTRY_IMAGE:$GIT_COMMIT_SHORT-jenkins-$CURRENT_BUILD_NUMBER'
                }
            }
        }

    }

}
