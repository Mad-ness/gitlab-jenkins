#!groovy

node {
  stage("Checkout") {
     checkout scm
  }

  stage("Stage1") {
    echo "${message}"  //access to your parameter
    sh "whoami"
    sh "cat /etc/fstab"
  }

  stage("Approve") {
    input "Do you accept changes?"
  }

  stage("Stage2") {
    sh "true"
  }

}

