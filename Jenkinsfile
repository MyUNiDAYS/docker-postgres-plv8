def build_docker(postgres, postgres_version, plv8){
    node('docker'){
        def image = 'postgres-plv8:' + postgres_version + '-' + plv8
        def app

        stage('Build Image'){
            checkout scm
            sh 'docker build -t ' + image + ' '  + postgres + '/' + plv8
        }

        stage('Test Image'){
            sh 'docker run -d --name postgres ' + image
            sh 'sleep 3'
            sh 'while ! docker exec -it postgres pg_isready -U postgres -h 127.0.0.1; do echo "waiting for database to start"; sleep 1; done'
            sh 'docker exec -it postgres psql -U postgres -c CREATE EXTENSION plv8; DO $$ plv8.elog(WARNING, plv8.version) $$ LANGUAGE plv8 | grep "${VERSION#????}"'
         }

    }
}


node('docker') {
    stage('Build Docker'){
        build_docker('9.5', '9.5.12', '2.1.0')
    }
}