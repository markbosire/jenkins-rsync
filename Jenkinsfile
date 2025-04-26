pipeline {
    agent {label 'agent'}

    environment {
        GIT_REPO = 'https://github.com/markbosire/jenkins-rsync.git'
        BASE_DIR = '/home/vagrant/rscync'
        GIT_CREDENTIALS = 'gittoken'
        DEFAULT_BRANCH = 'dev' // Specify your default branch here
    }

    stages {
        stage('Checkout Repository') {
            steps {
                // Checkout the repository with specified branch and credentials
                git credentialsId: GIT_CREDENTIALS, url: GIT_REPO, branch: DEFAULT_BRANCH
            }
        }
        
        stage('Check Jenkins User') {
            steps {
                sh 'whoami'
            }
        }
	stage('Install rsync') {
            steps {
                sh 'sudo apt install rsync'
            }
        }

        stage('Sync Branches') {
            steps {
                script {
                    // Define the branches and their corresponding directories
                    def branches = [
                        'prod': 'prod',
                        'dev': 'dev',
                        'test': 'test'
                    ]

                    // Loop through each branch and sync with its directory
                    branches.each { branch, dir ->
                        sh """
                            git checkout ${branch}
			    mkdir -p ${BASE_DIR}/${dir}/
                            rsync -avz --delete --exclude='.git/' ./ ${BASE_DIR}/${dir}/
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up the workspace
            deleteDir()
        }
    }
}
