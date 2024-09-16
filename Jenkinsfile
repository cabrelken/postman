pipeline {
    agent   {
        docker {
            image  'node:18'
        }
    }
    stages {
        stage('Install dependencies') {
            steps {
                sh 'npm install newman'
            }
        }

         stage('Install dependencies') {
            steps {
                sh 'newman -version'
            }
        }
        stage('Run Newman tests') {
            steps {
                // Exécuter les tests API avec Newman et générer des rapports au format CLI, JSON et JUnit
                sh '''
                newman run testAvecToken.postman_collection.json \
                 --reporters cli,json,junit \
                --reporter-json-export /etc/newman/report.json \
                --reporter-junit-export /etc/newman/report.xml
                '''
            }
        }
    }
    post {
        always {
            // Archiver les rapports générés pour les consulter après l'exécution du pipeline
            archiveArtifacts artifacts: 'report.json, report.xml'
        }
        success {
            echo 'API Tests Passed!'  // Affiche un message de succès si les tests réussissent
        }
        failure {
            echo 'API Tests Failed. Check the reports for details.'  // Message en cas d'échec
        }
    }
} 


//C:/laragon/www/postman/newman/Produits.postman_collection.json
//C:\laragon\www\postman\Produits.postman_collection.json
