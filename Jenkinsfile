def build_docker(postgres, postgres_version, plv8){
    node("docker"){
        checkout scm
        def image = "postgres-plv8:${postgres_version}-${plv8}"
        def repo = "unidays/postgres-plv8"
        def tag = "${postgres_version}-${plv8}"

        withCredentials([[$class: "UsernamePasswordMultiBinding", credentialsId: "21fdc354-dd19-4eb7-a274-0b25cd206756",
        usernameVariable: "DOCKER_USER", passwordVariable: "DOCKER_PASS"]]) {
            stage("Build Image"){
                checkout scm
                sh "docker build -t ${image} ${postgres}/${plv8}"
            }

            stage("Test Image"){
                sh "docker run -d --name postgres ${image}"
                sh "sleep 3"
                sh "while ! docker exec postgres pg_isready -U postgres -h 127.0.0.1; do echo 'waiting for database to start'; sleep 1; done"
                sh 'docker exec postgres psql -U postgres -c CREATE EXTENSION plv8; DO $$ plv8.elog(WARNING, plv8.version) $$ LANGUAGE plv8 | grep "${VERSION#????}"'
            }

            stage("Push Image"){
                sh "docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}"
                sh "docker build -t ${tag} ${postgres}/${plv8}"
                sh "docker tag ${tag} ${repo}:${tag}"
                sh "docker push ${repo}"

            }
        }
    }
}


node("docker"){
    stage("Build Docker"){
        parallel(
            "9.5.10": { build_docker("9.5", "9.5.10", "1.4.4") },
            "9.5.12": { build_docker("9.5", "9.5.12", "2.1.0") },
            "9.6.8": { build_docker("9.6", "9.6.8", "2.1.0") }
        )        
    }
}