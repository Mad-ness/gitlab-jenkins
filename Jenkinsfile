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
        sh "cd ansible; ANSIBLE_VAULT_PASSWORD=\"`~/bin/vault-env`\" ansible-playbook site.yml --syntax-check"
    }

    stage("TestBuild") {
        sh "cd ansible; ANSIBLE_VAULT_PASSWORD=\"`~/bin/vault-env`\" ansible-playbook site.yml --tags=install,uninstall"
    }

    stage("Approve") {
        input "The instance is ready to be deployed. Continue?"
    }

    stage("Deploy") {
        sh "cd ansible; ANSIBLE_VAULT_PASSWORD=\"`~/bin/vault-env`\" ansible-playbook site.yml --tags=install"
    }

}

