pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'build stage'
        sh 'mvn -DskipTests clean package'
        archiveArtifacts '**/target/*.jar'
      }
    }

    stage('test') {
      parallel {
        stage('smoke') {
          steps {
            echo 'test d\'intégration'
            sh 'mvn -Dtest="com.exemple.testingweb.smoke.**" test'
            junit '**/target/surefire-reports/TEST-*.xml'
          }
        }

        stage('Integration') {
          steps {
            echo 'test fonctionnel'
            sh 'mvn -Dtest="com.example.testingweb.integration.**" test'
          }
        }

        stage('Functional') {
          steps {
            echo 'smoke test'
            sh 'mvn -Dtest="com.example.testingweb.functional.**" test'
          }
        }

      }
    }

    stage('deploy') {
      steps {
        echo 'stage deploy'
        input 'Voulez - vous continuer? '
        sh 'mvn -DskipTests install'
      }
    }

  }
}