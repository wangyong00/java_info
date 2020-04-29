pipeline {
  agent any
  stages {
    stage('step1') {
      parallel {
        stage('step1') {
          steps {
            sh 'echo "hello"'
            sleep(time: 100, unit: 'SECONDS')
          }
        }

        stage('step1.1') {
          steps {
            catchError(buildResult: '1', message: '1', stageResult: '1') {
              echo '1'
            }

          }
        }

      }
    }

    stage('step2') {
      steps {
        build(job: 'java', propagate: true, quietPeriod: 5, wait: true)
      }
    }

  }
}