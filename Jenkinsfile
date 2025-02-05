pipeline {
    agent any
    
    environment {
        PHP_VERSION = '8.1'
        DISTRIB_CODENAME = 'bookworm' // Remplacer par la version exacte de Debian si nécessaire
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
                    
                    # Ajout du dépôt PHP en utilisant le codename explicite
                    sudo add-apt-repository -y "deb https://packages.sury.org/php/ $DISTRIB_CODENAME main"
                    
                    # Ajouter la clé de sécurité pour le dépôt
                    sudo wget -qO /etc/apt/trusted.gpg.d/sury-keyring.gpg https://packages.sury.org/php/apt.gpg
                    
                    # Mettre à jour les sources
                    sudo apt update
                    
                    # Installer PHP et les dépendances
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
