# ViewSonic Deployment Setup Guide

This document provides instructions for setting up and deploying the ManageX School application to the ViewSonic AWS environment.

## Prerequisites

1. Access to ViewSonic AWS account
2. GitHub repository access
3. AWS CLI installed and configured
4. kubectl installed
5. Access to necessary AWS resources (ECR, EKS, etc.)

## Required Secrets

The following secrets must be added to the GitHub repository:

| Secret Name | Description |
|-------------|-------------|
| `VIEWSONIC_AWS_ACCESS_KEY_ID` | ViewSonic AWS Access Key ID for regular operations |
| `VIEWSONIC_AWS_SECRET_ACCESS_KEY` | ViewSonic AWS Secret Access Key for regular operations |
| `VIEWSONIC_AWS_REGION` | ViewSonic AWS Region (e.g., ap-south-1) |
| `VIEWSONIC_AWS_ACCESS_KEY_ID_PROD` | ViewSonic AWS Access Key ID for production operations |
| `VIEWSONIC_AWS_SECRET_ACCESS_KEY_PROD` | ViewSonic AWS Secret Access Key for production operations |

## AWS Resources Setup

### 1. ECR Repository Setup

1. Log in to the ViewSonic AWS Console
2. Navigate to the ECR service
3. Create a new repository named `viewsonic`
4. Note the repository URI for later use

```bash
# Creating repository via AWS CLI
aws ecr create-repository --repository-name viewsonic --region ap-south-1 --profile viewsonic
```

### 2. EKS Cluster Configuration

Ensure the EKS cluster is properly configured:

```bash
# Update kubeconfig for ViewSonic's cluster
aws eks update-kubeconfig --name viewsonic-production-cluster --region ap-south-1 --profile viewsonic
```

### 3. Set Up Kubernetes Namespaces and Resources

```bash
# Create the namespace if it doesn't exist
kubectl create namespace viewsonic-production

# Apply base Kubernetes manifests
kubectl apply -k k8s/overlays/viewsonic
```

## Deployment Process

### Manual Deployment

1. Go to GitHub Actions
2. Select the "Deploy to Production" workflow
3. Click "Run workflow"
4. Enter the required inputs:
   - Deployment password
   - Branch name to deploy
   - Tag for the image (e.g., Version-1.0)
   - Check "Deploy To ViewSonic Production"
5. Click "Run workflow"

### CI/CD Pipeline Explanation

The CI/CD pipeline will:

1. Build the Docker image
2. Push the image to both ManageX and ViewSonic ECR repositories
3. Deploy first to the ManageX production environment
4. Then deploy to the ViewSonic production environment
5. Send Slack notifications for deployment status

## Kubernetes Configuration

ViewSonic's Kubernetes configuration is stored in `k8s/overlays/viewsonic/` and includes:

- Customized deployment configurations
- Service configurations
- Ingress rules
- Storage settings

## Troubleshooting

### Common Issues

1. **Deployment Fails with Authentication Errors**:
   - Verify GitHub secrets are correctly set
   - Check AWS IAM permissions

2. **Image Pull Failures**:
   - Ensure ECR permissions are correct
   - Verify image tags are correctly formatted

3. **Kubernetes Apply Errors**:
   - Check the kustomization.yaml file for correct syntax
   - Verify that all referenced resources exist

### Logs and Monitoring

Access logs through:

1. GitHub Actions workflow logs
2. AWS CloudWatch logs
3. Kubernetes logs:
   ```bash
   kubectl logs -n viewsonic-production deployment/viewsonic-school-production
   ```

## Maintenance

### Updating the Deployment

To update the ViewSonic deployment:

1. Make changes to the codebase
2. Push changes to the appropriate branch
3. Run the deployment workflow with "Deploy To ViewSonic Production" checked

### Rolling Back Deployments

If a deployment fails or causes issues:

1. Find the previous successful deployment tag
2. Run the deployment workflow with that tag
3. Or use Kubernetes rollback:
   ```bash
   kubectl rollout undo deployment/viewsonic-school-production -n viewsonic-production
   ```

## Contact Information

For issues or questions related to ViewSonic deployments, contact:

- DevOps Team: devops@greatify.ai
- ViewSonic Technical Contact: [Insert ViewSonic contact] 