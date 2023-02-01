pipeline {
    agent any
    environment {
        registryURI = "registry.hub.docker.com/"
        dev_registry = "prakuldip/jenkinsfile_kul_app_trialtwo"
        qa_registry = "prakuldip/qa_jenkinsfile_kul_app_trialthree_withfunction"
        dev_registryCredential = "dev-dh-cred"
        qa_registryCredential = "qa-dh-cred"
        imageTag = "b822a9dfd11973f4040263f06e0b00d6be0d7088"
    }
stages {
        stage('image pull, tag and push with function') {
            environment{
              SRC_IMAGE  = "${registryURI}${dev_registry}:${imageTag}"
              DEST_IMAGE = "${registryURI}${qa_registry}:${imageTag}"
              SRC_REGISTRY_CRED = "${dev_registryCredential}"
              DST_REGISTRY_CRED = "${qa_registryCredential}"
            }
            steps{
              imagePullTagPush(SRC_IMAGE,DEST_IMAGE,SRC_REGISTRY_CRED,DST_REGISTRY_CRED)
              sh "docker image rm '${SRC_IMAGE}' '${DEST_IMAGE}'"
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
def imagePullTagPush(String SRC_IMAGE, String DEST_IMAGE, String SRC_REGISTRY_CRED,String DST_REGISTRY_CRED) {
    def image_to_pull = docker.image(SRC_IMAGE)
    docker.withRegistry("https://${registryURI}",SRC_REGISTRY_CRED){
    image_to_pull.pull()
    }
    sh "docker image tag '${registryURI}${dev_registry}:${imageTag}' '${registryURI}${qa_registry}:${imageTag}'"
    def image_to_push = docker.image(DEST_IMAGE)
    docker.withRegistry("https://${registryURI}",DST_REGISTRY_CRED){
    image_to_push.push()
    }
}