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
                            execCommand: 'ls -la /tmp',
                            execCommand: '/usr/bin/systemctl stop train-schedule',
                            execCommand: 'rm -rf /opt/train-schedule/*',
                            execCommand: 'unzip /tmp/trainSchedule.zip -d /opt/train-schedule',
                            execCommand: '/usr/bin/systemctl start train-schedule'
                        )
                    ]
                    )
                ]
            )
        }

        }
    }
}