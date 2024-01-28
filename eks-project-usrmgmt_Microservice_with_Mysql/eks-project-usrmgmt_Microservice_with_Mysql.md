**Eks Project - Usermanagement Microservice with Mysql** 

**database** 

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.001.jpeg)

**• We are going to use EBS CSI Driver and use EBS Volumes for persistence storage to MySQL Database** 

**Kubernetes Important Concepts for Application Deployments** 



|Storage Class  - for persistant volume||
| - | :- |
|Persistent Volume Claim - to claim the volume||
|Config Map - config file for sql schema||
|Deployment, Environment Variables, Volumes, VolumeMounts||
|ClusterIP Service - for my sql server||
|Deployment, Environment Variables  - define db data||
|NodePort Service - for user managemant service||
|Secrets - for db password||
|Init Containers - it will wait for my sql pod to comeup then connect to usermgnt Microservice||
|Liveness & Readiness Probes - to check the pod status||
|Requests & Limits - for the pod limits||
|Namespaces - for the isolated resource to create||
|Resource quota - how much resource can be utilised in the namespace||

Create Cluster:

eksctl create cluster --name=eksdemo1 \

`                      `--region=us-east-1 \

`                      `--zones=us-east-1a,us-east-1b \                       --without-nodegroup 

eksctl utils associate-iam-oidc-provider \     --region us-east-1 \

`    `--cluster eksdemo1 \

`    `—approve

Create Public Node Group:   

eksctl create nodegroup --cluster=eksdemo1 \                        --region=us-east-1 \

`                       `--name=eksdemo1-ng-public1 \                        --node-type=t3.medium \

`                       `--nodes=2 \

`                       `--nodes-min=2 \

`                       `--nodes-max=4 \

`                       `--node-volume-size=20 \

`                       `--ssh-access \

`                       `--ssh-public-key=kube-demo \                        --managed \

`                       `--asg-access \

`                       `--external-dns-access \

`                       `--full-ecr-access \

`                       `--appmesh-access \

`                       `--alb-ingress-access 

In node-group security group allow all traffic and save

To Install ebs csi driver 

Create iam policy ec2 full access for ebs\_csi\_driver and attach to the node group iam role 

` `Deploy EBS CSI Driver

kubectl apply -k “github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/ kubernetes/overlays/stable/?ref=master"

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.002.png)

Check the ebs csi driver pods installed in kube-system namespace

Kube manifests: 

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.003.jpeg)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.004.png)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.005.png)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.006.png)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.007.jpeg)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.008.jpeg)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.009.png)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.010.png)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.011.jpeg)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.012.jpeg)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.013.jpeg)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.014.jpeg)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.015.jpeg)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.016.jpeg)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.017.jpeg)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.018.jpeg)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.019.jpeg)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.020.jpeg)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.021.jpeg)

![](Aspose.Words.6ad430fd-d958-4e82-b2c6-68835dc267e9.022.jpeg)
