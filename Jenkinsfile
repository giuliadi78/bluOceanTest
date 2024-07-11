pipeline {
    agent any
    
    stages {
        stage('Clona il repository') {
            steps {
                checkout scm
                sh 'pwd && ls -la'
            }
        }

        stage('Stop containers') {
            steps {
                script {
                    def command = 'systemctl --user stop container-frontend.service'
                    def result = sh(script: "sudo -u devops ${command}", returnStatus: true)
                }
            }
        }

        stage('Pulizia immagine') {
            steps {
                script {
                    def imageTag = 'localhost/frontend:1.0'

                    // Verifica se l'immagine esiste
                    def imageExists = sh(script: "sudo -u devops podman inspect --format='{{.Id}}' $imageTag", returnStatus: true) == 0

                    // Rimuovi l'immagine solo se esiste
                    if (imageExists) {
                        sh "sudo -u devops podman rmi $imageTag"
                    } else {
                        echo "L'immagine $imageTag non esiste. Nessuna rimozione necessaria."
                    }
                }
            }
        }

        stage('Compila e costruisci il container frontend') {
            steps {
                script {
                    // Sposta nella directory del repository clonato
                    sh 'sudo -u devops podman build -f Containerfile_dev -t frontend:1.0 --layers=false --ulimit nofile=2048:2048 .'
                }
            }
        }

        stage('Start containers') {
            steps {
                script {
                    def command = 'systemctl --user start container-frontend.service'
                    def result = sh(script: "sudo -u devops ${command}", returnStatus: true)
                }
            }
        }
    }

    post {
        success {
            echo 'La pipeline è stata eseguita con successo!'
        }
        failure {
            echo 'La pipeline è fallita. Controlla i log per i dettagli.'
        }
    }
}
