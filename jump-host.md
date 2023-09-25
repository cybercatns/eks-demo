# Execute below scripts in the jump host EC2 instance to configure a new cluster. 

## Install kubectl
```curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.27.5/2023-09-14/bin/linux/amd64/kubectl && chmod a+x kubectl && sudo cp kubectl /usr/bin/ && rm -f kubectl && kubectl version --short --client ```

## Install eksctl
```ARCH=amd64 && PLATFORM=$(uname -s)_$ARCH && curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$PLATFORM.tar.gz"```

```tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz && sudo mv /tmp/eksctl /usr/local/bin```

## Create EKS Cluster
1. Create cluster without a node group
```eksctl create cluster --name eks-demo --region us-east-1 --without-nodegroup```

2. Create IAM OIDC provider
```eksctl utils associate-iam-oidc-provider  --region us-east-1  --cluster eks-demo  --approve```

3. Create node group
```
eksctl create nodegroup --cluster=eks-demo \
 --region=us-east-1 \
 --name=eks-demo-node-group-01 \
 --node-type=t3.small \
 --nodes-min=1 \
 --nodes-max=3 \
 --ssh-access \
 --ssh-public-key=/home/ec2-user/.ssh/authorized_keys \
 --managed \
 --asg-access \
 --node-private-networking
```

4. List EKS clusters
```eksctl get cluster --region=us-east-1```

5. List NodeGroups in a cluster
```eksctl get nodegroup --cluster=eks-demo --region=us-east-1```

## Configure kubectl
```eksctl utils write-kubeconfig --cluster=eks-demo --region=us-east-1```

## Cleanup - DELETE Cluster
```eksctl delete cluster --name eks-demo --region=us-east-1```