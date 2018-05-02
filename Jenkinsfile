pipeline {
    agent any
    stages {
        stage('test_npvr_recording_deletion_5.1') {
            agent any
            steps {
                build(job: 'test_npvr_recording_deletion_5.1', propagate: false)
            }
        }

        stage('test_rsdvr_recording_deletion_5.1') {
            agent any
            steps {
                build(job: 'test_rsdvr_recording_deletion_5.1', propagate: false)
            }
        }

        stage('delete_recordings_copy_xml') {
          agent any
          environment {
            target_custer = '10.65.182.11'
          }
          steps {
            sh 'scp -p root@$target_cluster:/tmp/delete_recordings/*.xml .'
          }
        }

        stage('publish_junit') {
          steps {
            junit(testResults: '*.xml', healthScaleFactor: 1, allowEmptyResults: true)
          }
        }

    }
}
