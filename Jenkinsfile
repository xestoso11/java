pipeline {
    agent {
        docker { 
            image 'maven:3.9.6-amazoncorretto-8' // Imagen Docker con Maven para compilar Java
            args '-v /root/.m2:/root/.m2' // Montar el volumen del repositorio Maven local
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package' // Compilar el código Java usando Maven
            }
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    docker.build('javahellow:last') // Nombre de la imagen Docker y etiqueta
                    docker.withRegistry('http://localhost:8081/repository/helloword/', '9e1a3627-6b89-3843-96a5-d6c07306f7c9') {
                        docker.image('javahellow:last').push() // Subir la imagen a Nexus
                    }
                }
            }
        }
    }
}