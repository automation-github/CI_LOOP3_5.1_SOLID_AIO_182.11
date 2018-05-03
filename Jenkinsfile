pipeline {
    agent any
    environment {
      target_cluster = '10.65.182.11'
    }
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

        stage('test_npvr_playout_deletion_5.1') {
            agent any
            steps {
                build(job: 'test_npvr_playout_deletion_5.1', propagate: false)
            }
        }

        stage('test_rsdvr_playout_deletion_5.1') {
            agent any
            steps {
                build(job: 'test_rsdvr_playout_deletion_5.1', propagate: false)
            }
        }

        stage('copy_xmls') {
          agent any
          steps {
            sh 'scp -p root@$target_cluster:/tmp/delete_recordings/*.xml .'
          }
        }

        stage('publish junit results') {
          steps {
            junit(testResults: '*.xml', healthScaleFactor: 1.0, allowEmptyResults: true)
          }
        }
}
