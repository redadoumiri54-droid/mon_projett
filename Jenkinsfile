pipeline {
    agent {
        docker {
            image 'python:3.11-slim'
            args '--user root'
        }
    }

    stages {
        stage('Clonage') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-token',
                    url: 'https://github.com/redadoumiri54-droid/mon_projett.git'
            }
        }

        stage('Installation des dépendances') {
            steps {
                sh '''
                    python3 --version
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Linting') {
            steps {
                sh 'pip install flake8 && flake8 . --max-line-length=120 || true'
            }
        }

        stage('Tests unitaires') {
            steps {
                sh 'pip install pytest && pytest --junitxml=results.xml || true'
            }
        }

        stage('Sécurité SAST - Bandit') {
            steps {
                sh 'pip install bandit && bandit -r . -f xml -o bandit-report.xml || true'
            }
        }

        stage('Vérification dépendances - Safety') {
            steps {
                sh 'pip install safety && safety check || true'
            }
        }
    }

    post {
        success { echo '✅ Pipeline réussi.' }
        failure { echo '❌ Pipeline échoué.' }
    }
}
