pipeline {
    agent any
    
    parameters {
        string(name: "1315a")
        password(name: "qwe654dsa123")
    }

    stages {
        stage ('Preparing terraform') {
            steps {
               sh '''
                    cd terraform
                    cp .terraformrc ~/
               '''
            }
        }

        stage ('Provisioning for build instance') {
            steps {
                sh '''
                    cd terraform
                    terraform init
                    terraform plan 
                    terraform apply                    
                '''
            }
        }

        stage ('Adding host') {
            steps {
                sh '''
                    cd ../ansible
                    ansible-playbook common.yml                                      
                '''
            }
        }

        stage ('Build project') {
            steps {
                sh '''
                    cd ansible
                    echo "username=$username\npassword=$password\n" > docker.properties
                    ansible-playbook build.yml
                '''

        stage ('Deploy project') {
            steps {
                sh '''
                    cd ansible
                    ansible-playbook deploy.yml
                '''
            }
        }

    }
}