pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'ghcr.io/hotio/bazarr'
        CONTAINER_NAME = 'bazarr'
        PUID = '1000'
        PGID = '1000'
        UMASK = '002'
        TZ = 'America/Monterrey'
        WEBUI_PORTS = '6767/tcp,6767/udp'
        CONFIG_PATH = '/home/docker/bazarr/config'     
    }

    stages {
        stage('Deploy Docker Image') {
            steps {
                script {
                    sh """
                    docker run -d \
                        --restart always \
                        --name ${CONTAINER_NAME} \
                        -p 6767:6767 \
                        -e PUID=${PUID} \
                        -e PGID=${PGID} \
                        -e UMASK=${UMASK} \
                        -e TZ=${TZ} \
                        -e WEBUI_PORTS=${WEBUI_PORTS} \
                        -v ${CONFIG_PATH}:/config \
                        -v /media/Media/Movies:/Movies \
                        -v /media/Media/MyMovies:/MyMovies \
                        -v /media/Media/TVShows:/TVShows \                  
                        -v /media/Media/MyTVShows:/MyTVShows \
                        ${DOCKER_IMAGE}
                    """
                }
            }
        }
    }
}
