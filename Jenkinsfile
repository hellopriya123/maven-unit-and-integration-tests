pipeline{
    agent any
    stages{
        stage("Preparation"){
            steps{
                echo "Checkout Started"
                git branch: 'master', credentialsId: 'webhooktoken', url: 'https://github.com/hellopriya123/maven-unit-and-integration-tests.git'
                echo "Checkout completed"
     
            }
        }
        stage("Build"){
            steps{
             sh'''
            ls -lart
            mvn clean install
            '''
            echo "Build Automation"
            }
        }
         stage("Test"){
            steps{
            echo "All test cases are executed and is in success status"
            }
        }
        stage("Notification"){
            steps{
             emailext body: 'Build is successful', subject: 'Build Status', to: 'dhanapriyab6@gmail.com'
            }
        }
        
        }
    }
