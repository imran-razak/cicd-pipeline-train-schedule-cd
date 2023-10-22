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
        stage('DeployToStaging') {
            when {
                branch 'master'
            }
            steps {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'production',
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: '/var/lib/jenkins/jobs/train-schedule-cd/branches/master/builds/21/archive/dist/trainSchedule.zip',
                                        removePrefix: '/var/lib/jenkins/jobs/train-schedule-cd/branches/master/builds/21/archive/dist/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'clear'
                                    )
                                ]
                            )
                        ]
                    )
            }
        }
    }
}
