#!/usr/bin/env groovy

node('master') {
    try {
        stage('build') {
            // Checkout the app at the given commit sha from the webhook
            checkout scm

            // Install dependencies, create a new .env file and generate a new key, just for testing
            sh "composer install"
            sh "cp .env.ci .env"
            sh "mysql -uroot -proot123 -e 'CREATE SCHEMA IF NOT EXISTS fundishci DEFAULT CHARACTER SET utf8 ;'"
            //sh "mysql -uroot -proot123 -e 'GRANT ALL ON fundishci.* TO 'webserver'@'%' IDENTIFIED BY 'fE8ikgHc';"
            sh "php artisan key:generate"
            sh "php artisan migrate:fresh --seed"
            sh "php artisan passport:client --personal --name fundish-api"

            // Run any static asset building, if needed
        }

        stage('test') {
            // Run any testing suites
            sh "./vendor/bin/phpunit"
        }

        stage('deploy') {
            // If we had ansible installed on the server, setup to run an ansible playbook
            // sh "ansible-playbook -i ./ansible/hosts ./ansible/deploy.yml"
            sh "echo 'WE ARE DEPLOYING'"
        }
    } catch(error) {
        throw error
    } finally {
        // Any cleanup operations needed, whether we hit an error or not
    }

}