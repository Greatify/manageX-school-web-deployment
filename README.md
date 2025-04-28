# ManageX School Web Deployment

This repository contains the deployment configuration and infrastructure as code for the ManageX School Web application.

## Project Structure

```
.
├── .github/workflows/    # CI/CD pipeline configurations
│   ├── cd-dev.yaml      # Development deployment workflow
│   ├── ci-cd-stage.yaml # Staging deployment workflow
│   └── ci-cd-prod.yaml  # Production deployment workflow
└── k8s/                 # Kubernetes manifests
    ├── base/            # Base Kubernetes configurations
    │   ├── configmap/   # Configuration files
    │   ├── cronjob/     # Scheduled tasks
    │   ├── deployment/  # Main application deployment
    │   ├── hpa/        # Horizontal Pod Autoscaler
    │   ├── ingress/    # Ingress configurations
    │   ├── service/    # Service definitions
    │   └── secretproviderclass/ # Secret management
    └── overlays/       # Environment-specific configurations
        └── viewsonic-prod/  # ViewSonic production specific configs
            └── storage/     # Storage configurations for ViewSonic
```

## Detailed Component Overview

### 1. CI/CD Workflows (.github/workflows/)

#### Development Workflow (cd-dev.yaml)
- Triggered on pushes to development branches
- Builds and deploys to development environment
- Automated testing and validation

#### Staging Workflow (ci-cd-stage.yaml)
- Pre-production testing environment
- Quality assurance and integration testing
- Mirrors production configuration

#### Production Workflow (ci-cd-prod.yaml)
- Protected by deployment password
- Supports multiple production environments
- Features:
  - Concurrent deployment protection
  - Environment-specific validations
  - Image tagging for versioning
  - Selective deployment to School/ViewSonic environments

### 2. Kubernetes Configuration (k8s/)

#### Base Configuration (k8s/base/)
1. **ConfigMaps**
   - `base-nginx-configmap.yaml`: Nginx server configurations
   - `base-server-configmap.yaml`: Application server settings

2. **Deployment**
   - Main application deployment configuration
   - Container specifications
   - Environment variables

3. **Horizontal Pod Autoscaler (HPA)**
   - Automatic scaling based on metrics
   - Resource utilization thresholds

4. **Ingress**
   - External access rules
   - Load balancing configuration
   - SSL/TLS settings

5. **Service**
   - Internal service discovery
   - Port mappings
   - Load balancing

6. **Secret Provider Class**
   - External secrets management integration
   - Secure credential handling

#### Environment Overlays (k8s/overlays/)
- Environment-specific customizations
- Production-specific configurations
- ViewSonic-specific storage solutions

## Security

- Deployments are password-protected using GitHub Secrets
- Environments are isolated using GitHub Environments
- Concurrent deployments are managed to prevent conflicts
- Basic secrets management through Kubernetes secrets

## Infrastructure Configuration

- Horizontal Pod Autoscaler (HPA) for automatic scaling
- Environment-specific storage configurations
- Standard Kubernetes service definitions
- Namespace-based isolation for different environments

## Setup Instructions

### Prerequisites
- Kubernetes cluster access
- kubectl configured
- GitHub Actions secrets configured
- Required cloud provider access

### Development Deployment
1. Push changes to development branch
2. CI/CD pipeline automatically triggers
3. Monitor deployment in development environment

### Production Deployment
1. Create a new release tag
2. Navigate to GitHub Actions
3. Select "Deploy to Production" workflow
4. Provide required parameters:
   - Deployment password
   - Target branch
   - Production tag
   - Environment selection (School/ViewSonic)
5. Monitor deployment progress

## Maintenance and Troubleshooting

### Common Issues
1. **Storage Issues**
   - Check PVC status
   - Verify storage class availability

2. **Deployment Failures**
   - Check pod logs
   - Verify resource availability
   - Check network policies

## Best Practices

1. **Version Control**
   - Use semantic versioning for releases
   - Maintain clear commit messages
   - Follow branching strategy

2. **Deployment**
   - Always deploy to staging first
   - Use meaningful production tags
   - Monitor deployment metrics
   - Maintain deployment documentation

3. **Configuration Management**
   - Keep secrets in appropriate stores
   - Use ConfigMaps for non-sensitive data
   - Maintain environment-specific configurations
   - Document configuration changes

4. **Security**
   - Regular security audits
   - Keep dependencies updated
   - Monitor for vulnerabilities
   - Follow least privilege principle

## Contact and Support

For issues and support:
- Create GitHub issues for bugs
- Contact DevOps team for urgent issues
- Refer to internal documentation for detailed procedures
