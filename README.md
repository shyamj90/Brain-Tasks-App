# Setup Instructions

## 1. Create an EKS Cluster
- Create an **EKS Cluster** with a **Public Endpoint**.
- In the **EKS Cluster → Access tab**, add the **CodeBuild Deploy stage project’s IAM role**.
- Assign the **EKS admin policy** to this role.  
  This enables the CodeBuild project to deploy manifests to the EKS cluster.

## 2. Enable CloudWatch Logs
- Enable **CloudWatch logs** in EKS to monitor:
  - API authentication
  - Resource utilization
  - Application logs

## 3. Tag Public Subnets
- Tag your **Public Subnet** with:
  - Key: `kubernetes.io/role/elb`
  - Value: `1`  
  This makes the subnet available to the Load Balancer.

## 4. Configure CodePipeline
Set up **3 stages** in CodePipeline:

### Source
- Fetch the source code.
- Install the **GitHub app** in AWS and connect it to your repo.

### Build
- Build and upload the Docker image to **ECR**.
- Configure the appropriate environment variables:
  ```bash
  $ECR_REPO_URL

### Deploy
- Deploy the Kubernetes manifests to the **EKS cluster**.
- Configure the appropriate environment variables:
  ```bash
  $KUBECTL_DWNLD_PATH

## 5. Monitor CodePipeline
- Enable **CloudWatch logs** for CodePipeline to monitor failures during any stage.

## 6. Verify Deployment
- When the application is deployed successfully in EKS:
  - You should see the **bt-app Deployment** running.
  - Pods will be created based on the number of replicas configured.
  - An **internet-facing LoadBalancer** will be created.
  - The LoadBalancer will start serving application traffic.
