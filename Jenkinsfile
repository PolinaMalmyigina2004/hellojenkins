pipeline {
    agent any
    
    parameters {
        // ВАЖНО: используйте латиницу для переменных окружения!
        string(name: 'STUDENT_NAME', defaultValue: 'Polina Malmygina', description: 'Имя студента')
        string(name: 'PORT', defaultValue: '8001', description: 'Порт')
    }
    
    stages {
        stage('Удаляем старые контейнеры и образы') {
            steps {
                script {
                    // Исправлено имя контейнера
                    sh "docker ps -a -q --filter name=hello-malmyigina-polina-container | xargs -r docker rm -f"
                    // Исправлено имя образа
                    sh "docker images -q student-malmyigina-polina-app | xargs -r docker rmi -f"
                }
            }
        }
        stage('Выгружаем код из репозитория') {
            steps {
                git branch: 'main', url: 'https://github.com/PolinaMalmyigina2004/hellojenkins.git'
            }
        }
        stage('Собираем docker image') {
            steps {
                script {
                    // Исправлено имя образа
                    dockerImage = docker.build("student-malmyigina-polina-app")
                }
            }
        }
        stage('Запускаем тесты в докере') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'python -m unittest test_app.py'
                    }
                }
            }
        }
        stage('Запускаем докер контейнер') {
            steps {
                script {
                    // Исправлены имена контейнера и образа
                    sh "docker run -d --name hello-malmyigina-polina-container -p ${params.PORT}:${params.PORT} -e STUDENT_NAME='${params.STUDENT_NAME}' -e PORT=${params.PORT} student-malmyigina-polina-app"
                }
            }
        }
    }
}
