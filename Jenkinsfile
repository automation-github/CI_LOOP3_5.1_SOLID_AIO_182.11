pipeline {
  agent any
  environment {
    target_cluster = '10.65.182.11'
  }
  stages {
    stage('installation') {
      steps {
            sh 'python2.7 /var/tmp/Nightswatch/deploy/upgrade_machines_sa.py -p $target_cluster --ininame \'/var/lib/jenkins/nightswatch/5.1/config.ini\''
          }
    }

    stage('delete_recordings') {
      agent any
      steps {
        build (job: 'delete_recordings/master', propagate: false)
      }
    }

    stage('i_frame_manifest') {
      agent any
      steps {
        build (job: 'i_frame_manifest/master', propagate: false)
      }
    }

    stage('shared_segmenter') {
      agent any
      steps {
        build (job: 'shared_segmenter/master', propagate: false)
      }
    }

    stage('copy xmls') {
      steps {
        sh '''scp -p root@$target_cluster:/tmp/Shared_Segmenter/*.xml .
scp -p root@$target_cluster:/tmp/Iframe_manifest/*.xml .
scp -p root@$target_cluster:/tmp/delete_recordings/*.xml .'''
      }
    }

    stage('check_cores') {
      steps {
        sh 'ssh root@$target_cluster "python2.7 /var/Nightswatch/deploy/find_cores.py"'
      }
    }

    stage('publish junit results') {
      steps {
        junit(testResults: '*.xml', healthScaleFactor: 1.0, allowEmptyResults: true)
      }
    }

  }
}
