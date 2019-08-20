def bucket = 'my-lambda-s3-bucket'
def region = 'us-east-1'

pipeline {
    agent {
      docker {
        image 'pahud/aws-sam-cli:latest'
        args '-u root --privileged'
      }
    }
    options { timestamps () }
    environment {
      SAM_CLI_TELEMETRY=0
      AWS_DEFAULT_REGION="${region}"
      AWS_S3_BUCKET="${bucket}"
    }
    stages {
      stage('sam version') {
        steps {
          sh 'sam --version || exit 1'
        }
      }
      stage('sam validate'){
        steps {
          sh 'for dir in `find . -mindepth 1 -type d | grep -v git`; do sam validate -t $dir/sam.yaml; done'
        }
      }
      stage('sam package'){
        steps {
          sh 'for dir in `find . -mindepth 1 -type d | grep -v git | sed "s/.\\///g"`; do sam package --template-file ./$dir/sam.yaml --s3-bucket $AWS_S3_BUCKET --s3-prefix $dir --output-template-file ./$dir/packaged.yaml --debug; done'
        }
      }
      stage('sam deploy'){
        steps {
          sh 'for dir in `find . -mindepth 1 -type d | grep -v git | sed "s/.\\///g"`; do sam deploy --template-file ./$dir/packaged.yaml --stack-name $dir --capabilities CAPABILITY_IAM --debug; done'
        }
      }
    }
}
