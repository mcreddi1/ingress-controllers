# Ingress Controller

```
eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster expense \
    --approve
```

```
curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.10.0/docs/install/iam_policy.json
```

```
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam-policy.json
```

```
eksctl create iamserviceaccount \
--cluster=expense \
--namespace=kube-system \
--name=aws-load-balancer-controller \
--attach-policy-arn=arn:aws:iam::390403901513:policy/AWSLoadBalancerControllerIAMPolicy \
--override-existing-serviceaccounts \
--region us-east-1 \
--approve
```

```
helm repo add eks https://aws.github.io/eks-charts
```

```
helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=expense --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller
```


```
once above steps completed
```

```
w have to create a deployment set followed by service to access the pods and then create ingress resource to route the traffic
```
```
For ingress resource we need to create ACM in aws and then create R53 records for validation
then in ingress resource we need to choose the type of routing and ports to listen, etc.... we need to choose them based on our requirements
```

```
Finally we need to create ALB in spec of ingress resource
```

```
As a last step we need to take DNS from EC2 load balancer and create a name record with that dns choose alias to AppLB and region then choose the load balancer for ingress 
```