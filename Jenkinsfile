pipeline {
    agent any
    environment {
        registryURI = "registry.hub.docker.com/"
        dev_registry = "prakuldip/jenkinsfile_kul_app_trialtwo"
        qa_registry = "prakuldip/qa_jenkinsfile_kul_app_trialtwo"
        registryCredential = "dockerhub_cred"
        imageTag = "b822a9dfd11973f4040263f06e0b00d6be0d7088"
    }
stages {
        stage('image pull') {
            steps{
                script {
                    def image_to_pull = docker.image("${registryURI}${dev_registry}:${imageTag}")
                    docker.withRegistry("https://${registryURI}",registryCredential){
                    image_to_pull.pull()
                    }
                    sh "docker image tag '${registryURI}${dev_registry}:${imageTag}' '${registryURI}${qa_registry}:${imageTag}'"
                    def image_to_push = docker.image("${registryURI}${qa_registry}:${imageTag}")
                    docker.withRegistry("https://${registryURI}",registryCredential){
                    image_to_push.push()
                    }  /* pull tag and push code is successful*/
                }
            }
        }
    }
    post { 
        always { 
            echo 'Deleting Workspace'
            deleteDir() /* clean up our workspace */
        }
    }
}