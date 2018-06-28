#!groovy


stage('Checkout') {

    node('master') {
        checkout scm
        stash includes: 'ansible/**/**', name: 'ansiblePlaybooks'
    }

    node('ansible') {
        dir('.') {
            unstash 'ansiblePlaybooks'
        } 
    } 

}


node('ansible') {

    stage("SyntaxCheck") {
        sh "cd ansible; ansible-playbook site.yml --syntax-check"
    }

    stage("TestBuild") {
        sh "cd ansible; ansible-playbook site.yml --vault-password-file=${HOME}/etc/vault.passwd --tags=uninstall"
        sh "cd ansible; ansible-playbook site.yml --vault-password-file=${HOME}/etc/vault.passwd --tags=install,uninstall"
    }

    stage("Approve") {
        input "The instance is ready to be deployed. Continue?"
    }

    stage("Deploy") {
        sh "cd ansible; ansible-playbook site.yml --vault-password-file=${HOME}/etc/vault.passwd --tags=install"
    }

}

