# This is to install apache using HELM.

#	Create a namespace named as apache-demo-namespace.
```kubectl create namespace apache-demo-namespace```

#	Create a service named as apache-demo-service, targetPort 80 to nodePort 30004.
```kubectl apply -f apache-demo-service.yaml --namespace=apache-demo-namespace```

#	Create a deployment named as apache-demo-deployment.
```kubectl apply -f apache-demo-deployment.yaml --namespace=apache-demo-namespace```

#	Set labels app to apache_demo_app.
