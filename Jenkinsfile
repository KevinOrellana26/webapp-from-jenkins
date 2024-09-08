pipeline {
    agent {
        label 'agent-1'
    }

    environment {
        DOCKER_IMAGE = "mi-primer-img" //nombre de la imagen de Docker
        DOCKER_CONTAINER = "mi-primer-app" //nombre que tendrá el contenedor
    }

    stages{
        stage('clonar repositorio'){
            steps {
                //clonar el código desde Git
                git branch: 'main', url: ''
            }
        }

        stage('construir imagen Docker'){
            steps {
                script {
                    //construir la imagen Docker usando el Dockerfile
                    sh 'docker build -t --no-cache $DOCKER_IMAGE .'
                }
            }
        }

        stage('ejecutar contenedor Docker'){
            steps {
                script {
                    //verificar si el contenedor ya está corriendo y detenerlo
                    sh '''
                    if [ "$(docker ps -q -f name=$DOCKER_CONTAINER)" ]; then
                        docker stop $DOCKER_CONTAINER
                        docker rm $DOCKER_CONTAINER
                    fi
                    '''

                    //ejecutar el contenedor Docker de la app en PHP
                    sh 'docker run -d --name $DOCKER_CONTAINER -p 8080:80 $DOCKER_IMAGE'
                }
            }
        }
    }

    //esta secuencia siempre se ejecuta sin importar el resultado
    post {
        always {
            //mostrar los contenedores que están corriendo
            sh 'docker ps'
        }

        success {
            echo 'Pipeline completado exitosamente!'
        }

        failure {
            echo 'El pipeline falló. Verificar errores.'
        }
    }
}