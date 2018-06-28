#!groovy

pipeline {

    agent { label 'ansible' }

    stage("Checkout") {
        checkout scm
    }

    stage("Verification") {
        sh "cd ansible; ANSIBLE_VAULT_PASSWORD=\"`~/bin/vault-env`\" ansible-playbook site.yml --syntax-check"
    }

    stage("TestBuild") {
        sh "cd ansible; ANSIBLE_VAULT_PASSWORD=\"`~/bin/vault-env`\" ansible-playbook site.yml --tags=install,uninstall"
    }

    stage("Approve") {
        input "The instance is ready to be deployed. Continue?"
    }

    stage("Stage2") {
        sh "cd ansible; ANSIBLE_VAULT_PASSWORD=\"`~/bin/vault-env`\" ansible-playbook site.yml --tags=install"
    }

}

