pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
        VENV_DIR = 'venv'
    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                git 'https://github.com/Divine15b/depot.git'
            }
        }

        stage('Créer un environnement virtuel') {
            steps {
                sh 'rm -rf ${VENV_DIR}'
                sh 'python3 -m venv ${VENV_DIR}'
            }
        }

        stage('Installer les dépendances') {
            steps {
                sh "./${VENV_DIR}/bin/pip install --upgrade pip"
                sh "./${VENV_DIR}/bin/pip install -r requirements.txt"
            }
        }

        stage('Tests unitaires') {
            steps {
                sh "./${VENV_DIR}/bin/pytest test_app.py"
            }
        }

        stage('Déploiement avec Ansible') {
            steps {
                sh "./${VENV_DIR}/bin/ansible-playbook -i inventory deploy.yml"
            }
        }
    }

    post {
        success {
            echo "✅ Clonage, Tests unitaires et Déploiement réussis !"
        }
        failure {
            echo "🚨 Le pipeline a échoué ! Vérifie les logs."
        }
    }
}
