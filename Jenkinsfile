#!groovy

pipeline {
    agent any
    environment {
        ANSIBLE_CMD='/usr/local/bin/ansible'
        PLAYBOOK_DIR='plays'
    }
    stages {
        stage('Checkout SCM') {
            steps {
                deleteDir()
                checkout scm
            }
        }
        stage('Initialisation') {
            steps {
                // Just checking that we've got ansible installed.
                sh '${ANSIBLE_CMD} --version'
            }
        }
        stage('Check ENV') {
            steps {
                run_ansible_playbook('${PLAYBOOK_DIR}/check_env.yml')
            }
        }
        stage('Deploy Application') {
            steps {
                // Demo purposes wait 60 secnds until deployment
                sh 'sleep 60'
                run_ansible_playbook('${PLAYBOOK_DIR}/nginx_deploy.yml')
            }
        }
    }
}

// This is what's required to run an ansible playbook using the ansible plugin.
// Run the plugin with the default jenkins ssh key, NOT the root key.
void run_ansible_playbook(playbook) {
    ansiblePlaybook(
        playbook: playbook,
        inventory: "inventory/hosts",
        colorized: true,
        // The documentation tells you it requires the ID but use the name.
        // fa == root ansible key
        credentialsId: "jenkins-demo-key",
        installation: "ansible2.7",
        // extraVars: [
        // ],
        extras: ('-vvv'),
    )
}
