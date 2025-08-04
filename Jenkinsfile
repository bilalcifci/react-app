pipeline {
    // Pipeline'ın çalışacağı Jenkins agent'ı (makine)
    // Bu aşamada sadece Docker komutları çalışacağı için 'any' yeterli.
    agent any

    // Ortam değişkenleri
    environment {
        // Docker imajının adı ve etiketi
        IMAGE_NAME = "react-app"
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        // Docker container'ının adı
        CONTAINER_NAME = "react-app-container"
    }

    stages {
        // Bu aşamada, Dockerfile'ı kullanarak imaj oluşturulur.
        // React uygulamasının derleme işlemi Dockerfile içinde halledilir.
        stage('Build Docker Image') {
            steps {
                script {
                    // Docker imajını oluşturma komutu
                    // -t: İmaj için etiket belirler.
                    // . : Dockerfile'ın mevcut dizinde olduğunu belirtir.
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        // Önceki container'ı durdurup yenisini çalıştırır
        stage('Run Docker Container') {
            steps {
                script {
                    // Mevcut container varsa durdur ve kaldır
                    // '|| true' komutu, container yoksa hata vermesini engeller.
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"

                    // Yeni Docker container'ını çalıştır
                    // -d: Container'ı arka planda (detached) çalıştırır.
                    // -p 3000:80: Local makinenin 3000 portunu container'ın 80 portuna yönlendirir.
                    // --name: Container'a bir ad verir.
                    sh "docker run -d -p 3000:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }
}