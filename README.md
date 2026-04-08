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
curl -sL "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_checksums.txt" | grep $PLATFORM | sha256sum –check
tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz
sudo install -m 0755 /tmp/eksctl /usr/local/bin && rm /tmp/eksctl


website url :    eksctl-io/eksctl: The official CLI for Amazon EKS

 check installed versions:
1. eksctl version
2. aws –version
3. kubectl version
 <img width="1176" height="263" alt="Screenshot 2026-04-04 171227" src="https://github.com/user-attachments/assets/f35041e9-b633-4710-a64f-7d23ff04c520" />

 ## Step 4: Install EKS cluster, node groups & attach IAM role and update cluster

 1. Cluster command:
   eksctl create cluster --name=EKS-1 --region=us-east-1 --zones=us-east-1a,us-east-1b --without-nodegroup
<img width="1824" height="408" alt="Screenshot 2026-04-04 170802" src="https://github.com/user-attachments/assets/2795e0c9-72f7-4a77-a52b-f6f5b599075f" />

2. Attach IAM role.
   eksctl utils associate-iam-oidc-provider --region us-east-1 --cluster EKS-1 --approve
     
3.	Create Node Group:
  eksctl create nodegroup --cluster=EKS-1 --region=us-east-1 --name=node2 --node-type=c7i-flex.large --nodes=3 --nodes-min=2 --nodes-max=4 --node-volume-size=20 --ssh-access --ssh-public-key=mykeypair --managed --asg-access --external-dns-access --full-ecr-access --appmesh-access --alb-ingress-access

<img width="1560" height="566" alt="Screenshot 2026-04-04 171046" src="https://github.com/user-attachments/assets/e3946ca3-5a15-47b0-b73b-a5c22e436fd0" />

4.Now update the cluster:
   aws eks update-kubeconfig --name EKS-1 --region us-east-1
 <img width="1179" height="63" alt="Screenshot 2026-04-04 170519" src="https://github.com/user-attachments/assets/09108191-397a-4770-a1ef-e7932a026a4b" />
 
# Step 5: Install Jenkins and acess the dashboard and also install the required pluggins and also install docker and git:
# Jenkins Commands:
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/rpm-stable/jenkins.repo
sudo yum upgrade
sudo yum install fontconfig java-21-openjdk
sudo yum install jenkins
sudo systemctl daemon-reload

sudo systemctl enable Jenkins
sudo systemctl start Jenkins
sudo systemctl status jenkins
<img width="1909" height="931" alt="Screenshot 2026-04-06 114121" src="https://github.com/user-attachments/assets/0b9211a8-f021-4dd7-bda0-9c3be388dd34" />
<img width="182" height="197" alt="Screenshot 2026-04-06 114202" src="https://github.com/user-attachments/assets/693ae552-0156-4ead-9ce4-58188ded8ed6" />
<img width="1747" height="308" alt="Screenshot 2026-04-06 150459" src="https://github.com/user-attachments/assets/8219f5ee-7bc1-489d-b59b-aa99af6cb466" />
<img width="1828" height="385" alt="Screenshot 2026-04-06 150538" src="https://github.com/user-attachments/assets/dd8f0467-18d5-4630-931a-9b7a0c8aa38f" />


Plugins to install in Jenkins:

1.	Plugin stage view
2.	Docker pipeline
3.	Kubernetes
4.	Kubernetes CLI
<img width="1911" height="840" alt="Screenshot 2026-04-06 124006" src="https://github.com/user-attachments/assets/7bf5c6a8-a38f-444e-83dd-72413a5aa946" />
<img width="1891" height="684" alt="Screenshot 2026-04-06 154848" src="https://github.com/user-attachments/assets/21527f32-df68-4092-9ee5-d72736d98650" />

 # Step 6: Now add the DockerHub credentials in Jenkins global credentials:
Path: docker hub credentials. Jenkins ->manage Jenkins -> credentials -> system -> global
<img width="725" height="812" alt="Screenshot 2026-04-06 124702" src="https://github.com/user-attachments/assets/1a619457-9a6a-43f4-8215-131a395b132b" />

# Step 7: In Kubernetes we have RBAC concept, by using RBAC we can create user and Service account.

User: If any person onboard into your project at that scenario on behalf of his name create user and give access to login. 
Service Account: If we want to integrate third party tools to Jenkins (github, github actions etc..) In that case we service accounts.
Note: Now create service account and for that do role & role binding, why means we are giving what kind of permissions to Jenkins like (creation of pod only, only deployment permissions, only volume permissions etc.…).

**Create name space & Service Account**
1. Namespace:
 kubectl create ns webapps 

2. Service Account:
vim svc-account.yml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
  namespace: webapps
<img width="648" height="266" alt="Screenshot 2026-04-06 141416" src="https://github.com/user-attachments/assets/ac445b5f-cfb4-4c0a-b908-d6885a2d8a4e" />
<img width="907" height="122" alt="Screenshot 2026-04-06 141441" src="https://github.com/user-attachments/assets/a76f67a2-e7ec-4767-86e4-fcf909376109" />

# Step: 8 :	Now create role & role-binding to this service account.
vim role.yml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
name: app-role
namespace: webapps
rules:
- apiGroups:
- ""
- apps
- autoscaling
- batch
- extensions
- policy
- rbac.authorization.k8s.io
resources:
- pods
- componentstatuses
- configmaps
- daemonsets
- deployments
- events
- endpoints
- horizontalpodautoscalers
- ingress
- jobs
- limitranges
- namespaces
- nodes
- pods
- persistentvolumes
- persistentvolumeclaims
- resourcequotas
- replicasets
- replicationcontrollers
- serviceaccounts
- services
verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]

Kubectl create -f role.yml
<img width="1436" height="258" alt="Screenshot 2026-04-06 142132" src="https://github.com/user-attachments/assets/d7ca8b80-0ed4-4976-a76f-0270ae376c44" />

Now rolebinding:
vim rolebinding.yml

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: app-rolebinding
  namespace: webapps
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: app-role
subjects:
  - namespace: webapps
    kind: ServiceAccount
    name: Jenkins

kubectl create -f rolebinding.yml
<img width="991" height="137" alt="Screenshot 2026-04-06 142725" src="https://github.com/user-attachments/assets/0a8e5fd0-8383-40cf-b44d-7b4ce503bb35" />

# Step: 9
	Now Jenkins have permissions and if Jenkins communicate with Kubernetes, we need to store credentials in Jenkins. Let us say we have 3 clusters in AWS account and Jenkins which cluster need to communicate and in which name space should be executed. For that we secrets.
Secrets will create credentials.

vim secret.yml

apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
  name: mysecretname
  namespace: webapps
  annotations:
    kubernetes.io/service-account.name: Jenkins

kubectl create -f secret.yml
<img width="740" height="144" alt="Screenshot 2026-04-06 143610" src="https://github.com/user-attachments/assets/98b0b15c-3670-4d78-b3d4-29d0e3414f3a" />

	To check the created secret:
Command: Kubectl get secret mysecretname -n webapps

Now describe the secret, we will get one encrypted token
Kubectl describe secret mysecretname  -n webapps
<img width="1829" height="563" alt="Screenshot 2026-04-06 144602" src="https://github.com/user-attachments/assets/00301c6d-35f3-4b55-ba99-b131963adbd7" />

=> Save this token in Jenkins credentials.
<img width="709" height="661" alt="Screenshot 2026-04-06 144859" src="https://github.com/user-attachments/assets/2d00e3b1-92a9-4ab9-8144-d3c1135ae9e4" />
<img width="709" height="798" alt="Screenshot 2026-04-06 145002" src="https://github.com/user-attachments/assets/73db7218-f71a-4501-a026-181c5762c99d" />
<img width="1915" height="940" alt="Screenshot 2026-04-07 231826" src="https://github.com/user-attachments/assets/8991de10-4a62-452f-a1e9-09af776df2a7" />

Note: Id : k8-token. Same name should be in Main Jenkins file.
Copy & paste the Api server endpoint url in Main Jenkins file.
<img width="1909" height="768" alt="Screenshot 2026-04-06 145343" src="https://github.com/user-attachments/assets/d9033c13-e4f3-4d02-abba-bfe7852e51c2" />
<img width="1915" height="585" alt="Screenshot 2026-04-06 145628" src="https://github.com/user-attachments/assets/d6283899-d2a4-47cf-b1a6-56f204f9d37b" />

# Step : 10 : The above whole process is Jenkins integrate with Kubernetes. Now create a multistage pipeline in Jenkins.
<img width="1903" height="938" alt="Screenshot 2026-04-06 145934" src="https://github.com/user-attachments/assets/66434f42-218b-4056-9bec-bd9a691158c0" />

=> Pipeline should be fail because of docker permissions. Give the below command.
      chmod 777 /var/run/docker.sock
 => Check the pipeline job and troubleshoot if any sevice will fail.     
 <img width="1919" height="953" alt="Screenshot 2026-04-06 170938" src="https://github.com/user-attachments/assets/1f44d8d1-d90f-4293-8caf-61f0ee8c7d19" />

# Step 11: 	Now check the Kubernetes objects which has created.
       Kubectl get po -n webapps   
       <img width="967" height="396" alt="Screenshot 2026-04-06 155445" src="https://github.com/user-attachments/assets/a22837ef-1089-41d3-9ced-18a831faca2c" />
       Kubectl get deploy -n webapps
       <img width="838" height="378" alt="Screenshot 2026-04-06 155721" src="https://github.com/user-attachments/assets/a3eb3185-473b-4c84-b551-929cf8061e35" />
       Kubectl get svc -n webapps
       <img width="1816" height="377" alt="Screenshot 2026-04-06 155801" src="https://github.com/user-attachments/assets/2954f729-3ceb-4640-974b-2c20228d5592" />
 # Step : 12 : Now finally access the application through DNS LoadBalancer
 <img width="1919" height="936" alt="Screenshot 2026-04-06 165509" src="https://github.com/user-attachments/assets/aeb85e6e-25ae-405d-9fac-6d224921f404" />






















 
