# Install kubectl
```curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.27.5/2023-09-14/bin/linux/amd64/kubectl && chmod a+x kubectl && sudo cp kubectl /usr/bin/ && rm -f kubectl && kubectl version --short --client ```

# Install eksctl
```ARCH=amd64 && PLATFORM=$(uname -s)_$ARCH && curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$PLATFORM.tar.gz"```

```tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz && sudo mv /tmp/eksctl /usr/local/bin```

# Configure kubectl
```eksctl utils associate-iam-oidc-provider  --region us-east-1  --cluster eks-demo  --approve```
```aws eks update-kubeconfig --region us-east-1 --name eks-demo```
```eksctl utils write-kubeconfig --cluster=eks-demo --region=us-east-1```
