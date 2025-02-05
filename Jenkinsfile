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
                    
                    # Récupérer le codename de la distribution Debian
                    DISTRIB_CODENAME=$(lsb_release -sc)

                    # Vérifier que nous avons bien récupéré un codename valide
                    if [ -z "$DISTRIB_CODENAME" ]; then
                      echo "Erreur: Impossible de récupérer le codename de la distribution"
                      exit 1
                    fi

                    # Ajouter le dépôt PHP avec le codename correct
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
