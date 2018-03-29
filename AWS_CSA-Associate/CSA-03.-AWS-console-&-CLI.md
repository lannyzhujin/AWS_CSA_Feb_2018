# Console

 - Forum
 - **Documentation**

# CLI
[AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/index.html)

```
[ec2-user@ip-172-31-87-22 ~]$ pip install --upgrade --user awscli
[ec2-user@ip-172-31-87-22 ~]$ aws configure
AWS Access Key ID [None]: AKIA**********C3EA
AWS Secret Access Key [None]: u7uPa*************bXBGRN5
Default region name [None]: us-east-1
Default output format [None]:
[ec2-user@ip-172-31-87-22 ~]$ aws ec2 describe-instances --instance-ids i-03e5******bbf40d8
{
    "Reservations": [
        {
            "Instances": [
                {
                    "Monitoring": {
                        "State": "disabled"
                    },
                    "PublicDnsName": "ec2-54-226-70-42.compute-1.amazonaws.com",
                    "State": {
                        "Code": 16,
                        "Name": "running"
                    },
                    "EbsOptimized": false,
                    "LaunchTime": "2018-03-23T09:25:31.000Z",
                    "PublicIpAddress": "54.226.70.42",
                    "PrivateIpAddress": "172.31.87.22",
                    "ProductCodes": [],

```