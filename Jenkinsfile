pipeline {
    agent any
    stages {
        stage('validate') {
            
            steps {
                sh 'echo "validating"'
                sh 'aws cloudformation validate-template --template-url https://coda-assessment-meghana.s3.amazonaws.com/s3-cfn.yml --region us-east-1'
                
            }
        }
        stage('build') {
            
            steps {
                sh 'echo "deploying"'
                sh 'aws --region us-east-1 cloudformation create-stack --stack-name testjenkins --template-url https://coda-assessment-meghana.s3.amazonaws.com/s3-cfn.yml'
                
            }
        }
    }
}
