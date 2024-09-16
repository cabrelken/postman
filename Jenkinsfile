pipeline {
    agent {
        docker {
            image 'postman/newman'
            args '-v $WORKSPACE:/etc/newman --entrypoint=""'  // Monte le répertoire Jenkins dans le conteneur Docker
        }
    }

    stages {
        stage('Install htmlextra Reporter') {
            steps {
                // Installer le reporter htmlextra globalement
                sh 'npm install -g newman-reporter-htmlextra'
            }
        }

        stage('Check Newman Installation') {
            steps {
                // Vérifier que Newman est bien installé et fonctionnel
                sh 'newman --version'
            }
        }

        stage('Run Newman tests') {
            steps {
                // Exécuter les tests API avec Newman et générer des rapports CLI, JSON, JUnit et HTML avec htmlextra
                sh '''
                newman run /etc/newman/testAvecToken.postman_collection.json \
                 --reporters cli,json,junit,htmlextra \
                --reporter-json-export $WORKSPACE/report.json \
                --reporter-junit-export $WORKSPACE/report.xml \
                --reporter-htmlextra-export $WORKSPACE/report.html
                '''
            }
        }
    }

    post {
        always {
            // Archiver les rapports JUnit et JSON
            junit 'report.xml'
            archiveArtifacts artifacts: 'report.json, report.xml, report.html'

            // Publier le rapport HTML avec htmlextra
            publishHTML(target: [
                reportDir: "$WORKSPACE",
                reportFiles: 'report.html',
                reportName: 'Newman HTML Report',
                keepAll: true,
                alwaysLinkToLastBuild: true,
                allowMissing: false
            ])
        }
        success {
            echo 'API Tests Passed!'
        }
        failure {
            echo 'API Tests Failed. Check the reports for details.'
        }
    }
}
