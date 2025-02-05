pipeline {
    agent any
    
    environment {
        PHP_VERSION = '8.1'
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
                    sudo add-apt-repository -y 'deb https://packages.sury.org/php/ $(lsb_release -sc) main'
                    sudo wget -qO /etc/apt/trusted.gpg.d/sury-keyring.gpg https://packages.sury.org/php/apt.gpg
                    sudo apt update
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

