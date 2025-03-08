pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-seed-repo.git'
            }
        }
        stage('Generate Pipeline') {
            steps {
                script {
                    def jobConfig = """
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
                            TVSHOWS_PATH = '/mnt/Media/TVShows'
                            MYSHOWS_PATH = '/mnt/Media/MyShows'
                            MOVIES_PATH = '/mnt/Media/Movies'
                            MYMOVIES_PATH = '/mnt/Media/MyMovies'
                        }

                        stages {
                            stage('Deploy Docker Image') {
                                steps {
                                    script {
                                        sh \"""
                                        docker run --rm \
                                            --name \${CONTAINER_NAME} \
                                            -p 6767:6767 \
                                            -e PUID=\${PUID} \
                                            -e PGID=\${PGID} \
                                            -e UMASK=\${UMASK} \
                                            -e TZ=\${TZ} \
                                            -e WEBUI_PORTS=\${WEBUI_PORTS} \
                                            -v \${CONFIG_PATH}:/config \
                                            -v \${TVSHOWS_PATH}:/TVShows \
                                            -v \${MYSHOWS_PATH}:/MyShows \
                                            -v \${MOVIES_PATH}:/Movies \
                                            -v \${MYMOVIES_PATH}:/MyMovies \
                                            \${DOCKER_IMAGE}
                                        \"""
                                    }
                                }
                            }
                        }
                    }
                    """

                    // Create a new job with the generated pipeline
                    pipelineJob('DeployDockerPipeline') {
                        definition {
                            cps {
                                script(jobConfig)
                                sandbox()
                            }
                        }
                    }
                }
            }
        }
    }
}
