pipeline {
    agent any

    environment {
        // تعريف اسم الصورة (Image Name)
        DOCKER_IMAGE = "my-python-app:latest"
        CONTAINER_NAME = "my-python-app-container"
    }

    stages {
        // 1. مرحلة الـ Checkout (سحب الكود من GitHub)
        stage('Checkout') {
            steps {
                echo 'Checking out code from Git...'
                checkout scm
            }
        }

        // 2. مرحلة الـ Build (بناء صورة الـ Docker وتجهيز البيئة)
        stage('Build') {
            steps {
                echo 'Building Docker Image...'
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        // 3. مرحلة الـ Test (تشغيل الـ Tests داخل الـ Container أو بيئة مؤقتة)
        stage('Test') {
            steps {
                echo 'Running Unit Tests...'
                script {
                    // تشغيل الـ pytest للتأكد من سلامة الكود
                    sh "docker run --rm ${DOCKER_IMAGE} pytest test_app.py"
                }
            }
        }

        // 4. مرحلة الـ Deploy (تشغيل التطبيق ليكون متاحاً للمستخدمين)
        stage('Deploy') {
            steps {
                echo 'Deploying Application...'
                script {
                    // إيقاف وحذف الحاوية القديمة إذا كانت موجودة لتجنب تضارب المنافذ
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
                    
                    // تشغيل الحاوية الجديدة في الخلفية (Detached Mode) على بورت 5000
                    sh "docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}"
                }
            }
        }
    }

    // التنظيف والأخطار (اختياري ولكن احترافي)
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }

	
