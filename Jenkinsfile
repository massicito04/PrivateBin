pipeline {
    agent any
    
    environment {
        PHP_VERSION = '8.2'
    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }
        }
        
        stage('Setup PHP & Dependencies') {
            steps {
                sh '''
                    sudo apt update
                    sudo apt install -y lsb-release ca-certificates apt-transport-https software-properties-common
                    
                    # Installer PHP depuis les dépôts Debian officiels
                    sudo apt install -y php${PHP_VERSION} php-cli php-mbstring unzip curl
                    
                '''
                sh 'curl -sS https://getcomposer.org/installer | php'
                sh 'php composer.phar install'
            }
        }
        
        stage('Run Tests') {
            steps {
                sh 'vendor/bin/phpunit'
            }
        }
    }
}
