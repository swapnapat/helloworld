DOCKER_GROUP = 'swapnapat'
DOCKER_IMAGE = 'helloworld'
DOCKER_REGISTRY_CREDENTIALS_ID = '4b33a1e9-1793-4b3e-b4d6-5b999539896b'
DOCKER_CONTAINER_NAME = 'HelloWorld'
GIT_URL = 'https://github.com/swapnapat/helloworld.git'
GIT_CREDENTIALS_ID = 'c893c7d2-8753-407e-b5a9-b88a110f2955'
node {
    def app
    try {
        stage('Preparation') {
            checkout scm
  //          checkout(
    //            [
      //              $class: 'GitSCM', 
       //             branches: [[name: 'pipeline']], 
        //            doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], 
          //          userRemoteConfigs: [
           //             [
            //                credentialsId: GIT_CREDENTIALS_ID, 
              //              url: GIT_URL
               //         ]
                 //   ]
       //         ]
         //   )
        }
        stage('Build Docker Image') {
            app = docker.build("${DOCKER_GROUP}/${DOCKER_IMAGE}:${env.BUILD_ID}")
        }
        stage('Publish Image') {
            withDockerRegistry([credentialsId: DOCKER_REGISTRY_CREDENTIALS_ID]) {
                app.push()
                app.push('latest')
            }
        }
        // stage('Deploy') {
        //     sh "docker rm $(docker ps --all --quiet)"
        //     sh "docker rm $(docker ps --quiet --filter status=exited)"
        //     withDockerRegistry([credentialsId: DOCKER_REGISTRY_CREDENTIALS_ID]) {
        //         sh "docker pull ${DOCKER_GROUP}/${DOCKER_IMAGE}"
        //         sh "docker run --detach --publish 80:3000 --name ${DOCKER_CONTAINER_NAME} ${DOCKER_GROUP}/${DOCKER_IMAGE}"
        //     }
        // }
        stage('Clean Up') {
            sh "docker images ${DOCKER_GROUP}/${DOCKER_IMAGE} --filter \"before=${DOCKER_GROUP}/${DOCKER_IMAGE}:${env.BUILD_ID}\" -q | xargs docker rmi -f || true"
        }
    } 
    catch (err) {
        currentBuild.result = 'FAILED'
        throw err
    } 
    finally {
        cleanWs()
    }
}
