# EC2
## How to connect to instance?
Create EC2 instance
Download key (technicarium.pem) to ~/AWSKeys
chmod 400 technicarium.pem

~/AWSKeys/connect.sh
ssh -i "technicarium.pem" ec2-user@52.59.35.215
