pipeline {
    agent {
        docker {
            image 'postman/newman'  // Utilise l'image Docker officielle de Newman
            args '-v $WORKSPACE:/etc/newman --entrypoint=""'  // Monte le répertoire Jenkins dans le conteneur Docker
        }
    }

    stages {
        stage('Check Newman Installation') {
            steps {
                // Vérifier que Newman est bien installé et fonctionnel
                sh 'newman --version'
            }
        }
        

        stage('Run Newman tests') {
            steps {
                // Exécuter les tests API avec Newman et générer des rapports
                sh '''
                newman run /etc/newman/testAvecToken.postman_collection.json \
                 --reporters cli,json,junit \
                --reporter-json-export $WORKSPACE/report.json \
                --reporter-junit-export $WORKSPACE/report.xml
                '''
                // Vérifier que les rapports sont bien générés
                sh 'ls -l $WORKSPACE'
            }
        }
    }

    post {
        always {
            // Archiver les rapports générés
            junit 'report.xml'  // Archive le rapport JUnit
            archiveArtifacts artifacts: 'report.json, report.xml'  // Archive les fichiers JSON et XML
        }
        success {
            echo 'API Tests Passed!'  // Affiche un message de succès si les tests réussissent
        }
        failure {
            echo 'API Tests Failed. Check the reports for details.'  // Message en cas d'échec
        }
    }
}
