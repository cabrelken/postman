pipeline {
    agent {
        docker {
            image 'postman/newman'  // Utilisation de l'image Node.js pour installer Newman

        }
    }
    
    
    stages {
       stage('Installer Newman') {
            steps {
                sh 'npm install newman'  // Installer Newman localement
            }
        }

        stage('Exécuter les collections Postman avec Newman') {
            steps {
                // Utiliser le chemin local vers l'exécutable Newman
                sh 'newman run testAvecToken.postman_collection.json -e port8010.postman_environment.json'
            }
        }

        
    }

    post {
        always {
            echo 'Affichage des résultats des tests'
        }
    }
}


//C:/laragon/www/postman/newman/Produits.postman_collection.json
//C:\laragon\www\postman\Produits.postman_collection.json
