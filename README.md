# aws-sam-jenkinsfile
# Gabriel Ramos

DIRECTORY STRUCTURE
./Jenkinsfile
./mylambda1/
  --mylambda1.py
  --sam.yaml
./mylambda2/
  --mylambda2.py
  --sam.yaml

* Steps
  - Create IAM Policy from aws_sam_policy.json and add to IAM User Account
    - sam requires a LOT of permissions, all of which are not fully documented by AWS. Please use caution when applying.
  - Create separate directory for each Lambda Function
  - Create sam.yaml for each Lambda Function
  - Run Jenkinsfile from Jenkins Installation

* Jenkinsfile Stages
  - sam version = Checks whether SAM is installed and displays the version.
  - sam validate = Validates all sam.yaml files in each directory.
  - sam package = Packages all files and creates packaged.yaml file in each directory.
  - sam deploy = Deploys each Lambda Function as a CloudFormation Template, updates alias to "live"
