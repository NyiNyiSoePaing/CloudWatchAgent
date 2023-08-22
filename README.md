# CloudWatch Agent on On-Premise device
##  1.Create IAM User with the required permissions

### Login AWS and Create IAM USer
1. Open the <b>IAM Console</b>
2. Click on Users on the left panel
3. Click on Add Users and Enter a user name of your choice
4. Check <b>Progammatic Access</b> and click Next:Permissions
5. Click on Attach existing policies directly
6. Check <b>CloudWatchAgentServerPolicy</b> from the search menu
7. Click Next:Tags, followed my Next:Review and finally Create User
8. Finally, save <b>the Access key ID and Secret Access key</b> in a secure location


## 2.Setup AWS CLI on OnPrimise Server
```bash
# Run as root
# install aws cli
apt install awscli

# Confgiure AWS Profile
aws configure  --profile AmazonCloudWatchAgent

aws_access_key_id = 111111111111111111
aws_secret_access_key = aaaaaaaaaaaaaaaaaaa
region = ap-southeast-1
```

## 3.Setup aws cloudwatch agent  on OnPrimise Server
```bash
wget https://amazoncloudwatch-agent.s3.amazonaws.com/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb
dpkg -i amazon-cloudwatch-agent.deb
```

vi /opt/aws/amazon-cloudwatch-agent/etc/common-config.toml
```json
[credentials]
   shared_credential_profile = "AmazonCloudWatchAgent"
   shared_credential_file = "/root/.aws/credentials"
```

```bash

wget https://raw.githubusercontent.com/NyiNyiSoePaing/CloudWatchAgent/main/config.json -O /root/config.json

/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m onPremise -s -c file:/root/config.json

/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m onPrimise -a status

service amazon-cloudwatch-agent restart
```

## 3. Check Monitoring USage on Cloudwatch
1. Open the <b>CloudWatch Console</b>
2. Click on Metrics on the left panel

``` 
Metrics> All metrics> Custom namespaces > CWAgent
```

And Create Custom Dashbaord
