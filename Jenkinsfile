pipeline {
    agent any
    tools {
        // Asegúrate de haber configurado 'M3' en Manage Jenkins > Tools
        maven 'M3'
    }
    environment {
        // Obtenemos el token desde el baúl de contraseñas de Jenkins
        SONAR_TOKEN = credentials('sonarcloud-token-asgbuggy')
        
        // Tus datos exactos
        SONAR_ORG = 'eraf-2407'
        SONAR_PROJECT_KEY = 'ERAF-2407_devsecops-jenkins-k8s-tf-sast-sonarcloud-repo'
        SONAR_HOST_URL = 'https://sonarcloud.io'
    }
    stages {
        stage('Checkout Code') {
            steps {
                // Descarga el código del repositorio actual
                checkout scm
            }
        }
        stage('Build & Sonar Analysis') {
            steps {
                echo 'Compilando código y ejecutando análisis SonarCloud SAST...'
                
                // Ejecutamos Maven inyectando las variables de forma segura
                sh '''
                    export MAVEN_OPTS="-Xmx256m -XX:+UseSerialGC"
                    mvn clean verify sonar:sonar \
                      -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                      -Dsonar.organization=${SONAR_ORG} \
                      -Dsonar.host.url=${SONAR_HOST_URL} \
                      -Dsonar.token=${SONAR_TOKEN}
                      -Dsonar.verbose=true
                '''
            }
        }
    }
}
