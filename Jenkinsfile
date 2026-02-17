pipeline {
    agent any

    stages {
        // СТАДИЯ 1: Получение кода
        stage('Checkout Code') {
            steps {
                echo 'Забираем код из GitHub...'
                checkout scm
            }
        }

        // СТАДИЯ 2: Проверка файлов
        stage('Check Files') {
            steps {
                echo 'Проверяем созданные файлы...'
                bat '''
                    chcp 65001
                    echo === ФАЙЛЫ В РЕПОЗИТОРИИ ===
                    dir
                    echo.
                    echo === ПРОВЕРКА НАШИХ ФАЙЛОВ ===

                    if exist Dockerfile (
                        echo Dockerfile найден
                        echo Содержимое Dockerfile:
                        more +0 Dockerfile
                    ) else (
                        echo Dockerfile не найден
                    )

                    if exist requirements.txt (
                        echo requirements.txt найден
                    ) else (
                        echo requirements.txt не найден
                    )

                    if exist src\\app.py (
                        echo Файл src\\app.py найден
                    ) else (
                        echo Файл src\\app.py не найден
                    )

                    if exist .dockerignore (
                        echo .dockerignore найден
                    ) else (
                        echo .dockerignore не найден
                    )
                '''
            }
        }

        // СТАДИЯ 3: Теория Docker
        stage('Docker Theory') {
            steps {
                echo 'ТЕОРИЯ DOCKER'
                echo 'Если бы Jenkins не был на Windows, мы бы выполнили:'
                echo '1. docker build -t my-app .'
                echo '2. docker run -d -p 5000:5000 my-app'
                echo '3. curl http://localhost:5000/health'
                echo 'На Windows команды через bat будут работать, если Docker Desktop запущен.'
            }
        }

        // СТАДИЯ 4: Попробуем Docker команды
        stage('Try Docker Commands') {
            steps {
                echo 'Пробуем Docker команды...'
                bat '''
                    chcp 65001
                    echo Проверяем версию Docker
                    docker --version || echo Docker не доступен

                    echo Пробуем собрать Docker образ
                    docker build -t flask-lab2 . || echo Не удалось собрать образ

                    echo Пробуем запустить контейнер
                    docker run -d -p 5000:5000 --name flask-lab2-container flask-lab2 || echo Не удалось запустить контейнер
                '''
            }
        }
    }

    post {
        always {
            echo '=== ИТОГ РАБОТЫ ==='
            bat '''
                chcp 65001
                echo Дата: %date% %time%
                echo Процесс CI/CD понятен?
                echo 1. Код → GitHub
                echo 2. GitHub → Jenkins
                echo 3. Jenkins проверяет файлы
                echo 4. Docker теория изучена
            '''
        }
        success {
            echo 'ОТЛИЧНО! Вы поняли процесс CI/CD!'
            echo 'Даже если Docker не запускался — вы молодец!'
        }
        failure {
            echo 'Были ошибки, но это часть обучения!'
            echo 'Главное — файлы созданы и процесс понят.'
        }
    }
}
