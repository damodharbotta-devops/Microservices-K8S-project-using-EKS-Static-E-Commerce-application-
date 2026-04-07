# Microservices-K8S-project-using-EKS-Static-E-Commerce-application
## Step:1
Launch an Ec2 instance. 
Configuration: M7i-flex.large 28 GB RAM, Attach IAM role also to the server

<img width="1893" height="776" alt="Screenshot 2026-04-04 170616" src="https://github.com/user-attachments/assets/7d0a96ed-9a8e-4386-b2b5-3729bb7c8f04" />

## Step: 2 : Insatll required commands
1.	Install AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

website url:  https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

2.	Install Kubectl and EKSCTL

Kubectl:

curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.35.2/2026-02-27/bin/linux/amd64/kubectl
chmod +x  kubectl 
mv kubectl  /usr/local/bin


website url: Installing or updating to the latest version of the AWS CLI - AWS Command Line Interface

EKSCTL:

ARCH=amd64
PLATFORM=$(uname -s)_$ARCH

curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$PLATFORM.tar.gz"
# (Optional) Verify checksum
curl -sL "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_checksums.txt" | grep $PLATFORM | sha256sum –check
tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz
sudo install -m 0755 /tmp/eksctl /usr/local/bin && rm /tmp/eksctl


website url :    eksctl-io/eksctl: The official CLI for Amazon EKS

 check installed versions:
1. eksctl version
2. aws –version
3. kubectl version

   <img width="1176" height="263" alt="Screenshot 2026-04-04 171227" src="https://github.com/user-attachments/assets/f35041e9-b633-4710-a64f-7d23ff04c520" />
