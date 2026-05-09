def ejecutarMaven(String comando) {
    bat "mvn ${comando}"
}

pipeline {
    agent any

    stages {

        stage('Clonar código') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    ejecutarMaven('clean compile')
                }
            }
        }

        stage('Tests y Calidad en paralelo') {
            parallel {

                stage('Tests automáticos') {
                    steps {
                        script {
                            ejecutarMaven('test')
                        }
                    }
                }

                stage('Linter Java') {
                    steps {
                        script {
                            ejecutarMaven('checkstyle:checkstyle')
                        }
                    }
                }

                stage('Validación Maven') {
                    steps {
                        script {
                            ejecutarMaven('validate')
                        }
                    }
                }
            }
        }

        stage('Empaquetado') {
            steps {
                script {
                    ejecutarMaven('package -DskipTests')
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline ejecutado correctamente.'
        }

        failure {
            echo 'El pipeline falló. Revisar consola de Jenkins.'
        }
    }
}