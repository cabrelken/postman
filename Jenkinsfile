pipeline {
    agent {
        docker {
            image 'postman/newman:alpine'  // Using the official Newman Docker image
            args '-v $WORKSPACE:/newman'  // Mount Jenkins workspace to a writable directory
        }
    }
    
    
    stages {
       stage('Installer Newman') {
            steps {
                sh 'newman -version'  // Installer Newman localement
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
