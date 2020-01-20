# Common AWS CLI Command Cheatsheet

A list of common commands for working with the AWS CLI.

## Get information

### Retrieve information about the available AMIs

Export to file __amis.txt__. Search AMIs where the name contains the term **EKS** and owner is **amazon**.

`aws ec2 describe-images --owners amazon --filters 'Name=name, Values="*eks*" > ~/amis.txt`
