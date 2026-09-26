pipeline {
    agent any

    stages {
        stage('Получение кода') {
            steps {
                git branch: 'main', url: 'https://github.com/juliyurkova/jenkins-pipeline-test.git'
            }
        }
        stage('Сборка') {
            steps {
                echo "Сборка приложения..."
                sh 'exit 1'
            }
        }
        stage('Тестирование') {
            steps {
                echo "Тестирование приложения..."
            }
        }
        stage('Деплой на стейджинг') {
            steps {
                sh 'chmod u+x deploy smoke-tests'
                sh './deploy staging'
                sh './smoke-tests'
            }
        }
        stage('Деплой на продакшн') {
            steps {
                sh './deploy production'
            }
        }
    }

    post {
        always {
            echo "Пайплайн завершён"
        }
        success {
            echo "Пайплайн завершён успешно"
            mail to: 'julia17yurkova@mail.ru',
                 subject: "${env.JOB_NAME} - Сборка № ${env.BUILD_NUMBER} УСПЕШНА",
                 body: "Пайплайн успешно завершён. Ссылка: ${env.BUILD_URL}"
        }
        failure {
            echo "Пайплайн провален"
            mail to: 'ваша_почта@mail.ru',
                 subject: "${env.JOB_NAME} - Сборка № ${env.BUILD_NUMBER} ПРОВАЛЕНА",
                 body: "Пайплайн завершился с ошибкой. Проверьте: ${env.BUILD_URL}"
        }
        cleanup {
            cleanWs()
        }
    }
}
