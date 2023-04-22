pipeline {

  agent any

  environment {
    APP_NAME = 'andriodDemo'
  }

  options {
    // Stop the build early in case of compile or test failures
    skipStagesAfterUnstable()
  }

  stages {
    stage("Detect build type"){
      steps {
        echo "Building..."
      }
    }
  }
}
