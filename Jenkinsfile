pipeline {
    agent any
    
    environment {
        DB_URL = 'mysql+pmysql://usr:ptwd@host:3306/db'
        DISABLE_AUTH = true
    }
    
    stages {
        stage('Сборка') {
            steps {
                echo 'Сборка приложения...'
                sh '''
                    echo "Содержимое рабочей директории:"
                    ls -lh
                '''
                sh 'exit 1'
                echo "URL базы данных: ${DB_URL}"
                echo "DISABLE_AUTH: ${DISABLE_AUTH}"
                echo "Запуск задачи с номером сборки: ${env.BUILD_NUMBER} на ${env.JENKINS_URL}"
            }
        }
        
        stage('Тестирование') {
            steps {
                echo 'Тестирование приложения...'
                sh 'echo "Запуск тестов..."'
                sh 'echo "Все тесты пройдены успешно!"'
            }
        }
        
        stage('Деплой на стейджинг') {
            steps {
                echo 'Проверка наличия команд...'
                sh 'which chmod || echo "chmod not found"'
                echo 'Деплой на стейджинг...'
                sh 'chmod +x deploy'
                sh './deploy staging'
            }
        }
        
        stage('Проверка работоспособности') {
            steps {
                echo 'Запуск дымовых тестов...'
                sh 'chmod +x smoke-tests'
                sh './smoke-tests'
            }
        }
        
        stage('Деплой на продакшн') {
            steps {
                echo 'Деплой на продакшн...'
                sh './deploy production'
            }
        }
    }
    
    post {
        always {
            echo 'Этот блок выполняется всегда, независимо от статуса завершения'
        }
        success {
            echo 'Этот блок выполняется, если сборка успешна'
        }
        failure {
            echo 'Этот блок выполняется, если задача провалилась'
            mail to: 'julia17yurkova@mail.ru',
                 subject: "${env.JOB_NAME} - Сборка № ${env.BUILD_NUMBER} провалилась",
                 body: "Для получения дополнительной информации о провале пайплайна, проверьте консольный вывод по адресу ${env.BUILD_URL}"
        }
        unstable {
            echo "Это будет выполняться, если статус завершения был 'нестабильный'"
        }
        changed {
            echo 'Это будет выполняться, если состояние пайплайна изменилось'
        }
        fixed {
            echo 'Это будет выполняться, если предыдущий запуск был провальным, а сейчас успешный'
        }
    }
}
