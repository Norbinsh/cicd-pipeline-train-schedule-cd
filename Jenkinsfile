pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('DeployToStage') {
            when {
                branch 'master'
            }
            steps {
            sshPublisher(
                failOnError: true,
                continueOnError: false,
                publishers: [
                    sshPublisherDesc(
                        configName: 'staging',
                    transfers: [
                        sshTransfer(
                            sourceFiles: 'dist/trainSchedule.zip',
                            removePrefix: 'dist/',
                            remoteDirectory: '/tmp',
                            execCommand: 'sudo ls -la /tmp'
                            // execCommand: 'sudo /usr/bin/systemctl stop train-schedule'
                            // execCommand: 'sudo rm -rf /opt/train-schedule/*'
                            // execCommand: 'sudo unzip /tmp/trainSchedule.zip -d /opt/train-schedule'
                            // execCommand: 'sudo /usr/bin/systemctl start train-schedule'
                        )
                    ]
                    )
                ]
            )
        }

        }
    }
}