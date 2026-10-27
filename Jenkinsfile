pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Install') {
            steps {
                sh 'composer install --no-interaction'
                sh '[ -f .env ] || cp .env.example .env'
                sh 'php artisan key:generate'
            }
        }
        stage('Test') {
            steps { sh 'php artisan test' }
        }
        stage('Deploy') {
            steps {
                sh '''
                rsync -a --delete --exclude .git --exclude .env --exclude database/database.sqlite ./ /var/www/laravel-app/
                [ -f /var/www/laravel-app/.env ] || cp .env /var/www/laravel-app/.env
                cd /var/www/laravel-app
                touch database/database.sqlite
                php artisan migrate --force
                chmod -R 777 storage bootstrap/cache database
                '''
            }
        }
    }
}
