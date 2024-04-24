pipeline {
    agent {
        docker {
            // Utiliza la imagen Maven 3.9.6 con Amazon Corretto 8 como base
            image 'maven:3.9.6-amazoncorretto-8'
            // Monta el directorio de trabajo dentro del contenedor
            args '-v $PWD:/workspace'
        }
    }
    environment {
        // Define la variable de entorno JAVA_HOME para la compilación con Maven
        JAVA_HOME = '/usr/lib/jvm/java-1.8.0-amazon-corretto'
    }
    stages {
        stage('Checkout') {
            steps {
                // Clona el repositorio público de GitHub
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/main']], 
                          userRemoteConfigs: [[url: 'https://github.com/tu-usuario/tu-repositorio.git']]])
            }
        }
        stage('Build') {
            steps {
                // Ejecuta el comando 'mvn package' dentro del contenedor Docker
                script {
                    docker.image('maven:3.9.6-amazoncorretto-8').inside('-v $PWD:/workspace') {
                        sh 'mvn -B -DskipTests clean package'
                    }
                }
            }
        }
    }
    post {
        success {
            // Si la compilación tiene éxito, puedes realizar acciones adicionales aquí
            echo 'Compilación exitosa!'
        }
        failure {
            // Si la compilación falla, puedes manejar el error aquí
            echo 'La compilación ha fallado. Revisar los logs para más detalles.'
        }
    }
}
