pipeline{
    
    agent any
    tools{
        maven 'mymaven'
        terraform 'myterraform'
    }
    stages{
        stage('Build maven project'){
            steps{
               checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/chandanadevopslearn/Financeproject.git']])
                sh 'mvn clean install'
            }
        }
        stage('Test'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t mchandana123/devops-project .'
                }
            }
        }
        stage('push the docker image to hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerpwd')]) {
        sh 'docker login -u mchandana123 -p ${dockerpwd} '
    
}
        sh 'docker push  mchandana123/devops-project '
    }
                
            }
            
        }
        stage('terraform init & plan'){
            steps{
                script{
                    sh 'terraform init'
                    sh 'terraform plan -out tfplan'
                    sh 'terraform apply --auto-approve tfplan'
                    
                }
            }
            
        }
        
        stage('Execute Playbook'){
            steps{
               ansiblePlaybook credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'myansible', inventory: 'hosts', playbook: 'ansible-playbook.yml'
            }
        }
    }
}
