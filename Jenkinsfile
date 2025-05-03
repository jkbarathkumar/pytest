pipeline{
  agent any
  environment{
    VENV_DIR='venv'
  }
  stages{
    stage('Checkout'){
      steps{
        checkout scm
      }
    }
    stage('Set Up Virtual Environment'){
      steps{
        sh 'python3 -m venv $VENV_DIR'
        sh './$VENV_DIR/bin/pip install --upgrade pip'
        sh './$VENV_DIR/bin/pip install -r requirements.txt'
      }
    }
    stage('Run Tests with Coverage'){
      steps{
        sh './$VENV_DIR/bin/pytest -v bin/pytest/test --cov=app --cov-report=xml --cov-report=html'
      }
    }
    stage('Publish Coverage Report'){
      steps{
        publishHTML(target:[
          reportDir:'htmlcov',
          reportFiles:'index.html',
          reportName:'HTML Coverage Report'
        ])
      }
    }
  }
  post{
    always{
      archiveArtifacts artifacts:'coverage.xml', fingerprint:true
    }
  }
}
