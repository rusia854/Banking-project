pipeline{
    agent any
    stages{
        //stage('Checkout'){
          //  steps{
            //     git url: 'https://github.com/rusia854/Banking-project.git'
              //   echo 'github url checkout'
           // }
       // }
        stage('Compile'){
            steps{
                echo 'starting compiling'
                sh 'mvn compile'
            }
        }
        stage('Test'){
            steps{
                sh 'mvn test'
            }
        }
        stage('QA'){
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('Package'){
            steps{
                sh 'mvn package'
            }
        }
        stage('Build'){
          steps{
               sh 'docker build -t sumrus/banking-project:1.0 .'
           }
        }
        stage('Login'){
          steps{
               withCredentials([string(credentialsId: 'dockerhubpassword', variable: 'dockerhubpass')]) {
                sh 'docker login -u sumrus -p ${dockerhubpass}'
                }
           }
        }
        stage('Push'){
          steps{
              sh 'docker push sumrus/banking-project:1.0 '
           }
        }
        stage('Deploy'){
          steps{
             ansiblePlaybook become: true, credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', sudoUser: null, vaultTmpPath: ''
           }
        }
    }
}
