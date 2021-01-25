pipeline {


  agent any

  parameters {
    string(
      name: 'PARAMETER_FILE_PATH',
      defaultValue: 'parameters.yaml',
      description: 'aws cloudformation create-stack command extra arguments'
    )
    string(
      name: 'REGION',
      defaultValue: 'us-east-1',
      description: 'AWS CLI region name'
    )
    string(
      name: 'STACK_NAME',
      defaultValue: 'assessment$BRANCH_NAME',
      description: 'CloudFormation stack name'
    )
    string(
      name: 'TEMPLATE_FILE_PATH',
      defaultValue: 'blueprint.yaml',
      description: 'CloudFormation template file path'
    )
  }

  stages {
    stage('Initialize working directory') {
        steps {
          dir ("${env.WORKSPACE}") {
            cleanWs()
          }
        }
      }

    stage('Retrieve CloudFormation template file from Github') {
      steps {
        checkout scm
        }
      }
    stage('Validate Cloudformation Template') {
        steps {
            dir ("${env.WORKSPACE}") {
              sh """ aws cloudformation validate-template --template-body file://"${params.TEMPLATE_FILE_PATH}"
                """
            
            }
        }
      } 
  
    stage('Create Stack') {
      steps {
          dir ("${env.WORKSPACE}") {
            sh """if ! aws cloudformation describe-stacks --stack-name "${params.STACK_NAME}" ; then
              echo  "Creating stack" 
              aws cloudformation create-stack \
                --stack-name "${params.STACK_NAME}" \
                --template-body file://./"${params.TEMPLATE_FILE_PATH}" \
                --parameters file://./"${params.PARAMETER_FILE_PATH}" --capabilities CAPABILITY_NAMED_IAM
                else
                echo  "Stack exists, attempting update .."
                aws cloudformation update-stack \
                --stack-name "${params.STACK_NAME}" \
                --template-body file://./"${params.TEMPLATE_FILE_PATH}" \
                --parameters file://./"${params.PARAMETER_FILE_PATH}" --capabilities CAPABILITY_NAMED_IAM
                
              fi"""         
              }
          }
        }
      }
}

