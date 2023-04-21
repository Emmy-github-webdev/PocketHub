pipeline {
  agent any
  environment {
    APP_NAME = 'test'
  }
  options {
    // Stop the build early in case of compile or test failures
    skipStagesAfterUnstable()
  }
  stages {
   
    stage('Detect build type') {
        steps {
            echo "Detect the build branch..."
            script {
            if (env.BRANCH_NAME == 'develop' || env.CHANGE_TARGET == 'develop') {
                env.BUILD_TYPE = 'debug'
            } else if (env.BRANCH_NAME == 'master' || env.CHANGE_TARGET == 'master') {
                env.BUILD_TYPE = 'release'
            }
        }
    }

    stage('Compile') {
        steps {
            echo "Compile the app and its dependencies..."
            sh './gradlew compile${BUILD_TYPE}Sources'
        }
    }

    stage('Build') {
        steps {
            echo "Build the andriod application..."
            sh './gradlew assemble${BUILD_TYPE}'
            sh './gradlew generatePomFileForLibraryPublication'
        }
    }

    stage('Publish') {
        environment {
            APPCENTER_API_TOKEN = credentials('emmanuel-appcenter-api-token')
        }
        steps {
            appCenter apiToken: APPCENTER_API_TOKEN,
                    ownerName: 'emmanuel ogah',
                    appName: 'andriod-app-cicd',
                    pathToApp: '**/${APP_NAME}-${BUILD_TYPE}.apk',
                    distributionGroups: 'emmanuel ogah'
        }
    }

    // stage('Publish') {
    //     steps {
    //         echo "Archive the APKs so that they can be downloaded from Jenkins"
    //         archiveArtifacts "**/${APP_NAME}-${BUILD_TYPE}.apk"
    //         // Archive the ARR and POM so that they can be downloaded from Jenkins
    //         // archiveArtifacts "**/${APP_NAME}-${BUILD_TYPE}.aar, **/*pom-   default.xml*"
    //     }
    // }

  }
 }
}