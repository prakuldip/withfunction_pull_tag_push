pipeline {
    agent any
    environment {
        registryURI = "registry.hub.docker.com/"
        dev_registry = "prakuldip/jenkinsfile_kul_app_trialtwo"
        qa_registry = "prakuldip/qa_jenkinsfile_kul_app_trialtwo_withfunction"
        registryCredential = "dev-dh-cred"
        imageTag = "b822a9dfd11973f4040263f06e0b00d6be0d7088"
    }
stages {
        stage('image pull, tag and push with function') {
            environment{
              SRC_IMAGE  = "${registryURI}${dev_registry}:${imageTag}"
              DEST_IMAGE = "${registryURI}${qa_registry}:${imageTag}"
            }
            steps{
              imagePullTagPush(SRC_IMAGE,DEST_IMAGE)
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
def imagePullTagPush(string SRC_IMAGE, string DEST_IMAGE) {
    //def image_to_pull = docker.image("${registryURI}${dev_registry}:${imageTag}")
    def image_to_pull = docker.image(SRC_IMAGE)
    docker.withRegistry("https://${registryURI}",registryCredential){
    image_to_pull.pull()
    }
    sh "docker image tag '${registryURI}${dev_registry}:${imageTag}' '${registryURI}${qa_registry}:${imageTag}'"
    //def image_to_push = docker.image("${registryURI}${qa_registry}:${imageTag}")
    def image_to_push = docker.image(DEST_IMAGE)
    docker.withRegistry("https://${registryURI}",registryCredential){
    image_to_push.push()
    }
}