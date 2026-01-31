# Amazon EKS (Elastic Kubernetes Service) [In progress]
Amazon EKS (Elastic Kubernetes Service) is a managed Kubernetes service provided by AWS.

🌱 In simpler terms:
- Kubernetes is a system that helps you run and manage containerized applications.
- Amazon EKS takes care of setting up and managing Kubernetes for you, so you don’t have to install it yourself.
- EKS runs your Kubernetes control plane (brain of Kubernetes) on AWS in a secure and highly available way.

| Control Plane         | Worker Node           |
| --------------------- | --------------------- |
| Makes decisions       | Executes them         |
| Runs Kubernetes brain | Runs your apps (pods) |
| Managed by AWS in EKS | Can be EC2 or Fargate |

- Even though EKS manages the control plane, your application pods still need to run somewhere. That somewhere is usually `EC2`. It can also be
  `EC2` runs your application pods as worker nodes.

- When you create an EKS cluster, you usually set up:
> EKS Control Plane – Fully managed by AWS.
> Node Group (worker nodes) – This is a group of EC2 instances that run the Kubernetes worker nodes.

<img width="195" alt="image" src="https://github.com/user-attachments/assets/1d385444-fb31-494a-b0fe-80aa36689abf" />


Example flow: <br>
Let’s say you deploy a microservice to your EKS cluster:
- EKS sees a Deployment manifest and decides to run 3 pods.
- It looks at available EC2 worker nodes in the cluster.
- It places those pods on the EC2s with enough resources.
- The containers run inside pods on those EC2 instances.

You can view your EKS nodes (EC2s) in the AWS Console:
1. Go to EC2 → Instances
2. Filter by tag: eks:cluster-name=<your-cluster-name>

or, in terminal
`kubectl get nodes -o wide`

## ECS vs EKS?
