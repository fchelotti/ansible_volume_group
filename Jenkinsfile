pipeline {
    agent {
        label "mapfre-ansible"
    }

    parameters {
        string(defaultValue: "0.0.0.0", name: "Server")
    }

    stages {

        stage('Ansible Clone Repo') {
            steps {
                container('ansible') {
                    git credentialsId: 'bitbucket_token', url: 'https://github.com/fchelotti/ansible_volume_group.git', branch: 'develop'
                }
            }
        }

        stage('Pre Build') {
            steps {
                container('ansible') {
                    sh 'for ip in ${Server}; do sed -i "/linux/a $ip" Modulos/hosts; done'
                }
            }
        }

        stage('Apply Volume Group') {
            steps {
                withCredentials([string(credentialsId: 'root_password', variable: 'root_passwd')
                ]){
                container('ansible') {
                    script {
                        sh 'ansible-playbook -i Modulos/hosts Modulos/sda4.yaml -e "root_password=${root_passwd}"'
                    }
                }
                }
            } 
        }
    }
}