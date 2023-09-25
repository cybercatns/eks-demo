# Install kubectl
```curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.27.5/2023-09-14/bin/linux/amd64/kubectl && chmod a+x kubectl && sudo cp kubectl /usr/bin/ && rm -f kubectl && kubectl version --short --client ```

# Configure kubectl
```aws eks update-kubeconfig --region us-east-1 --name eks-demo```

