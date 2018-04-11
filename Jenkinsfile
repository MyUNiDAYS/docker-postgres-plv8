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