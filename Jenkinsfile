pipeline {
    agent any

    stages {
        stage('Checkout') {
             checkout scm 
        }        
       
        stage('Create virtual envirnment') {
            steps {
                sh '''#!/bin/bash
                    python3 -m venv venv
                 source ./venv/bin/activate'''
        
            }
        }
        stage('Install dependencies') {
            steps{
                sh '''#!/bin/bash
                source ./venv/bin/activate
                pip install -r requirements.txt'''
            }
        }    
                
        stage('migration') {
            steps{
                sh '''
                  . venv/bin/activate
                python3 manage.py migrate'''
            }
        }
        
        stage('Run test'){
            steps{
                sh '''
                  . venv/bin/activate
                  python3 manage.py test''' 
            }     
        }    
      stage('SonarQube Analysis') {
           def scannerHome = tool 'SonarScanner';
           withSonarQubeEnv() {
          sh "${scannerHome}/bin/sonar-scanner"
           }
       }
    }
}
