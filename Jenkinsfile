pipeline {
    agent { label 'ubuntu'}
    environment {
        CONTAINER = 'gomap'
        IMAGE = 'GOMAP'
        VERSION = '1.3.2'
    }
    
    stages {
        stage('Build') {
            when { changeset "singularity/*"}
            steps {
                sh '''
                    singularity --version && \
                    ls -lah && \
                    mkdir tmp && \
                    sudo singularity build --tmpdir tmp ${IMAGE}.sif singularity/Singularity.mpich-3.2.1
                '''
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh '''
                    ls -lh
                    singularity exec ${IMAGE}.sif ls
                '''
            }
        }
    }
    post{
                success{
                    echo 'Image Successfully tested'
                    azureUpload (storageCredentialId:'gomap', filesPath:"${IMAGE}.sif",allowAnonymousAccess:true, virtualPath:"${IMAGE}/${VERSION}/", storageType:"file",containerName:'gomap')
                    echo 'Image Successfully uploaded'
                }
            }
}
