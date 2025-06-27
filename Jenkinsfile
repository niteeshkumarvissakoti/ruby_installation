pipeline {
    agent any

    environment {
        REMOTE_HOST = "weblogic@10.22.124.96"  // Replace with actual user@ip
        OUTPUT_FILE = "ruby_install_status_${BUILD_NUMBER}.txt"
    }

    stages {
        stage('Install Ruby on Remote VM') {
            steps {
                sshagent(credentials: ['43abd6be-97dd-47b1-b11c-6d63b21356ca']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no $REMOTE_HOST '
                        if command -v ruby > /dev/null; then
                            echo "Ruby already installed"
                            ruby --version
                        else
                            echo "Installing Ruby..."
                            sudo yum install -y ruby
                        fi

                        echo "Installation completed successfully (build $BUILD_NUMBER)" > ~/ruby_install_status.txt
                    '
                    """
                }
            }
        }

        stage('Copy Status File Back (Optional)') {
            steps {
                sshagent(credentials: ['43abd6be-97dd-47b1-b11c-6d63b21356ca']) {
                    sh """
                    scp -o StrictHostKeyChecking=no $REMOTE_HOST:~/ruby_install_status.txt $OUTPUT_FILE
                    """
                }
            }
        }

        stage('Archive Status File') {
            steps {
                archiveArtifacts artifacts: "${OUTPUT_FILE}", allowEmptyArchive: false
            }
        }
    }
}
