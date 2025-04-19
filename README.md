# 🚀 GitOps Architecture with ArgoCD on AWS EKS

![AWS](https://img.shields.io/badge/AWS-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![EKS](https://img.shields.io/badge/EKS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![ArgoCD](https://img.shields.io/badge/ArgoCD-1B9AD9?style=for-the-badge&logo=argo&logoColor=white)
![Helm](https://img.shields.io/badge/Helm-0F1689?style=for-the-badge&logo=helm&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Splunk](https://img.shields.io/badge/Splunk-000000?style=for-the-badge&logo=splunk&logoColor=white)
![OpenTelemetry](https://img.shields.io/badge/OpenTelemetry-425CC7?style=for-the-badge&logo=opentelemetry&logoColor=white)

## 📋 Overview

This repository demonstrates a comprehensive GitOps implementation for managing applications on AWS EKS clusters using ArgoCD and Helm Charts. The architecture follows GitOps principles to maintain a declarative infrastructure and application deployment strategy, with built-in observability through Splunk OpenTelemetry Collector integration.

Key features include:
- Multi-environment support (Development, Production)
- Full application lifecycle management via GitOps
- Comprehensive observability with Splunk OpenTelemetry Collector
- AWS ALB-based ingress management with proper health checks
- Pre-deployment validation with sync health checks
- Automatic application instrumentation for observability

## 🔄 GitOps Architecture Diagram

```mermaid
flowchart TB
    subgraph "Developer Workflow"
        Dev[fa:fa-laptop "Developers"] -->|"Push changes"| Git[(fa:fa-code-branch "Git Repository")]
        Dev -->|"Clone / Pull"| Git
    end
    
    subgraph "GitOps Engine"
        Git -->|"Monitor"| ArgoCD["fa:fa-sync ArgoCD"]
        ArgoCD -->|"Apply changes"| K8S[" fa:fa-dharmachakra Kubernetes"]
        ArgoCD -->|"Report status"| Git
    end
    
    subgraph "Runtime Environment"
        K8S -->|"Deploy"| Apps["fa:fa-cubes Applications"]
        K8S -->|"Deploy"| Infra["fa:fa-server Infrastructure"]
        
        Collector["fa:fa-search Splunk
        OpenTelemetry
        Collector"] -->|"Send telemetry"| Splunk["fa:fa-chart-line Splunk
        Platform"]
        
        Apps -->|"Generate logs/metrics"| Collector
        Infra -->|"Generate logs/metrics"| Collector
    end
    
    subgraph "Environments"
        direction LR
        Dev_Env["fa:fa-flask Dev"]
        Prod_Env["fa:fa-industry Prod"]
    end
    
    ArgoCD -->|"Sync apps to"| Dev_Env
    ArgoCD -->|"Sync apps to"| Prod_Env
    Dev_Env -.->|"Deployed to"| K8S
    Prod_Env -.->|"Deployed to"| K8S
    
    subgraph "Helm Charts"
        direction LR
        App_Charts["fa:fa-file-code Application
        Charts"]
        Infra_Charts["fa:fa-cogs Infrastructure
        Charts"]
    end
    
    Git -->|"Contains"| App_Charts
    Git -->|"Contains"| Infra_Charts
    App_Charts -.->|"Used by"| ArgoCD
    Infra_Charts -.->|"Used by"| ArgoCD
        
    classDef git fill:#f34f29,color:white,stroke:#da5a47,stroke-width:1px
    classDef argocd fill:#329AD6,color:white,stroke:#2f90c5,stroke-width:1px
    classDef k8s fill:#326CE5,color:white,stroke:#2e64d4,stroke-width:1px
    classDef apps fill:#47A248,color:white,stroke:#3f9240,stroke-width:1px
    classDef infra fill:#767676,color:white,stroke:#666666,stroke-width:1px
    classDef splunk fill:#111111,color:white,stroke:#000000,stroke-width:1px
    classDef env fill:#FF9900,color:white,stroke:#ed8f00,stroke-width:1px
    classDef charts fill:#0F1689,color:white,stroke:#0d1476,stroke-width:1px
    classDef collector fill:#425CC7,color:white,stroke:#3c54b5,stroke-width:1px
    
    class Git git
    class ArgoCD argocd
    class K8S k8s
    class Apps apps
    class Infra infra
    class Splunk splunk
    class Dev_Env,Prod_Env env
    class App_Charts,Infra_Charts charts
    class Collector collector
```

## 🔄 The Power of GitOps Architecture

### What is GitOps?

GitOps is a paradigm shift in how we manage and deploy infrastructure and applications. At its core, GitOps uses Git as the **single source of truth** for declarative infrastructure and applications. With GitOps:

- **✓ Declarative** - Everything is defined as code (Infrastructure as Code)
- **✓ Versioned & Immutable** - Complete history of all changes
- **✓ Pulled Automatically** - Changes are pulled by operators, not pushed
- **✓ Continuously Reconciled** - System ensures actual state matches desired state

### Why GitOps Matters

Traditional deployment approaches often suffer from environment drift, manual errors, and lack of audit trails. GitOps solves these problems by:

1. **🔒 Improving Security**
    - Reduced direct access to production systems
    - Cryptographically verifiable audit trail of all changes
    - Role-based access controls through Git

2. **⚡ Accelerating Deployments**
    - Automated continuous delivery pipelines
    - Faster recovery from failures
    - Easier rollbacks to previous stable states

3. **🔍 Enhancing Visibility**
    - Complete audit history of all changes
    - Clear visibility into what's deployed where
    - Improved collaboration between teams

4. **🔄 Ensuring Consistency**
    - Eliminates drift between environments
    - Consistent state across all clusters
    - Self-healing infrastructure

### ArgoCD: The GitOps Engine

ArgoCD serves as the GitOps engine in our architecture, continuously synchronizing the desired state in Git with the actual state in Kubernetes. As a Kubernetes-native tool, ArgoCD:

- **🔄 Continuously monitors** Git repositories for changes
- **📊 Compares** the current state with the desired state
- **🔧 Automatically applies** necessary changes to reach desired state
- **🚨 Detects and alerts** on drift or synchronization failures
- **📱 Provides a UI dashboard** for visibility across all applications

### Architectural Benefits

Our implementation combines GitOps with Helm charts and multi-environment support to achieve:

1. **🌐 Environment Parity** - Dev and production environments use the same deployment process with environment-specific configurations.

2. **📦 Application Packaging** - Helm charts standardize application deployment patterns.

3. **🔄 Continuous Synchronization** - ArgoCD ensures the cluster state always matches the Git repository.

4. **📊 Observability Integration** - Splunk OpenTelemetry Collector provides comprehensive monitoring.

5. **🔄 Seamless Rollbacks** - In case of issues, reverting to a previous state is as simple as reverting a Git commit.

### Real-world Impact

Organizations implementing GitOps have reported:

- **⏱️ 80% reduction** in time to deploy
- **🔍 90% improvement** in recovery time
- **🔧 70% reduction** in configuration errors
- **📈 Significant increase** in deployment frequency

By making Git the single source of truth, this architecture provides a robust, audit-able, and scalable approach to managing Kubernetes infrastructure across multiple environments.

## 📂 Repository Structure

The repository follows a well-organized structure to manage multiple applications across development and production environments:

```plaintext
.
├── .gitignore                           # Git ignore patterns
├── README.md                            # This documentation file
│
├── HelmCharts/                          # All Helm charts for applications and infrastructure
│   ├── annotations                      # Shared annotations reference file
│   │
│   ├── splunk-otel-collector/           # Splunk OpenTelemetry Collector
│   │   ├── .helmignore
│   │   ├── Chart.yaml                   # Chart metadata
│   │   ├── values.yaml                  # Default values
│   │   └── templates/                   # Kubernetes manifest templates
│   │       ├── _helpers.tpl             # Helper functions for templates
│   │       ├── configmap.yml            # Collector configuration
│   │       ├── deployment.yaml          # Main collector deployment
│   │       ├── hpa.yaml                 # Horizontal Pod Autoscaler
│   │       ├── NOTES.txt                # Post-installation notes
│   │       ├── pre-sync-healthcheck.yaml # PreSync health check
│   │       ├── secret.yaml              # Secrets for Splunk token and endpoint
│   │       ├── service.yaml             # Service for the collector
│   │       └── serviceaccount.yaml      # Service account with RBAC
│   │
│   ├── app-client/                      # Frontend application chart
│   │   ├── .helmignore
│   │   ├── Chart.yaml                   # Chart metadata
│   │   ├── values.yaml                  # Default values
│   │   ├── values-dev.yaml              # Development-specific values
│   │   ├── values-prod.yaml             # Production-specific values
│   │   └── templates/                   # Kubernetes manifest templates
│   │       ├── _helpers.tpl             # Helper functions
│   │       ├── configmap.yml            # Environment variables
│   │       ├── deployment.yaml          # Application deployment
│   │       ├── hpa.yaml                 # Horizontal Pod Autoscaler
│   │       ├── ingress.yaml             # AWS ALB Ingress
│   │       ├── NOTES.txt                # Usage notes
│   │       ├── pre-sync-healthcheck.yaml # Health validation
│   │       ├── service.yaml             # Kubernetes service
│   │       └── serviceaccount.yaml      # Service account
│   │
│   └── app-api/                         # Backend API application chart
│       ├── .helmignore
│       ├── Chart.yaml                   # Chart metadata
│       ├── values.yaml                  # Default values
│       ├── values-dev.yaml              # Development-specific values
│       ├── values-prod.yaml             # Production-specific values
│       └── templates/                   # Kubernetes manifest templates
│           ├── _helpers.tpl             # Helper functions
│           ├── configmap.yml            # Environment variables
│           ├── deployment.yaml          # Application deployment
│           ├── hpa.yaml                 # Horizontal Pod Autoscaler
│           ├── ingress.yaml             # AWS ALB Ingress
│           ├── NOTES.txt                # Usage notes
│           ├── pre-sync-healthcheck.yaml # Health validation
│           ├── service.yaml             # Kubernetes service
│           └── serviceaccount.yaml      # Service account
│
├── eks-dev/                             # Development environment configuration
│   ├── applications/                    # Application definitions
│   │   ├── splunk-otel-collector.yaml   # Observability collector definition
│   │   ├── app-client.yaml              # Frontend application definition
│   │   └── app-api.yaml                 # Backend API application definition
│   └── root.yaml                        # Root application that manages all applications
│
└── eks-prod/                            # Production environment configuration
    ├── applications/                    # Production applications
    │   ├── splunk-otel-collector.yaml   # Production observability collector
    │   ├── app-client.yaml              # Production frontend application
    │   └── app-api.yaml                 # Production backend API application
    └── root.yaml                        # Production root application
```

## 🧩 Key Components

### 📦 Helm Charts

The `HelmCharts` directory contains all Helm charts for both applications and infrastructure. Each chart includes:

#### Application Charts (app-client, app-api)
These are your business applications deployed to the cluster:

- **Frontend (app-client)**
    - Web application (React, Angular, etc.)
    - Configured with NodeJS instrumentation for observability
    - Exposed via AWS ALB ingress
    - TCP health checks for availability validation

- **Backend API (app-api)**
    - API service (.NET Core, Node.js, Java, etc.)
    - Instrumented with OpenTelemetry for tracing and metrics
    - Configured with HTTP health checks
    - Exposed via AWS ALB ingress with path-based routing

#### Infrastructure Chart (splunk-otel-collector)
This is responsible for collecting and forwarding telemetry data:

- **Splunk OpenTelemetry Collector**
    - Collects logs from Kubernetes pods
    - Gathers metrics from Kubelet
    - Receives traces from instrumented applications
    - Forwards all telemetry to Splunk platform
    - Configured for auto-discovery of applications

### 📱 Environment Management

The architecture supports multiple environments through separate directories:

#### Development Environment (eks-dev)
- Contains ArgoCD applications targeting the development cluster
- Uses `values-dev.yaml` for environment-specific configurations
- Configured for internal access with appropriate security groups

#### Production Environment (eks-prod)
- Contains ArgoCD applications targeting the production cluster
- Uses `values-prod.yaml` for environment-specific configurations
- Configured with stricter security and higher resource requirements

### 🔧 Pre-Sync Health Checks

A notable feature is the pre-sync health checks implemented for each application:

- **API health checks**: HTTP-based validation for backend services
- **UI health checks**: TCP-based connection tests for frontend applications
- **Collector health checks**: Port availability validation for the collector

These checks run before ArgoCD applies changes, ensuring that only healthy applications are updated and preventing broken deployments.

### 🔒 Security Features

The architecture incorporates several security best practices:

- **Secret Management**: Splunk tokens stored in Kubernetes secrets
- **RBAC**: Service accounts with least-privilege permissions
- **Network Security**: AWS security groups control access
- **TLS**: HTTPS termination at ALB with modern TLS policies
- **Health Checks**: Prevent deployment of broken applications

## 🛠️ Setup and Usage

### Prerequisites

- AWS CLI configured with appropriate permissions
- kubectl installed and configured
- Helm 3.x installed
- ArgoCD CLI installed
- Splunk HEC token and endpoint information

### Initial Setup

1. **Clone the Repository**
   ```bash
   git clone https://github.com/your-username/your-gitops-repo.git
   cd your-gitops-repo
   ```

2. **Configure AWS CLI and Connect to EKS**
   ```bash
   aws configure
   aws eks update-kubeconfig --name your-cluster-name --region your-region
   ```

3. **Install ArgoCD on Your Cluster**
   ```bash
   kubectl create namespace argocd
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```

4. **Configure Splunk Token**

   Update the `values.yaml` in the `HelmCharts/splunk-otel-collector` directory:

   ```yaml
   # Splunk specific settings
   splunk:
     endpoint: "https://your-splunk-hec-endpoint.com/services/collector"
     token: "your-splunk-hec-token"
     index: "k8-dev"
   ```

5. **Apply the Root Application**

   This will bootstrap the entire GitOps process:

   ```bash
   kubectl apply -f eks-dev/root.yaml
   ```

6. **Access ArgoCD UI**

   ```bash
   kubectl port-forward svc/argocd-server -n argocd 8080:443
   ```

   Then open https://localhost:8080 in your browser.

### Adding New Applications

1. **Create a New Helm Chart**

   Add your chart in the `HelmCharts` directory:

   ```bash
   mkdir -p HelmCharts/your-new-app/templates
   ```

   Create the necessary files following the structure of existing charts:
    - Chart.yaml
    - values.yaml
    - values-dev.yaml
    - values-prod.yaml
    - Templates directory with resources

2. **Add ArgoCD Application Definition**

   Create a new file in `eks-dev/applications/your-new-app.yaml`:

   ```yaml
   apiVersion: argoproj.io/v1alpha1
   kind: Application
   metadata:
     name: your-new-app
     namespace: argocd
   spec:
     destination:
       name: in-cluster
       namespace: eks-dev
     source:
       path: "HelmCharts/your-new-app"
       repoURL: "git@github.com:your-username/your-gitops-repo.git"
       targetRevision: HEAD
     # Add other necessary configurations
   ```

3. **Add Observability Annotations**

   For automatic instrumentation, add the appropriate annotations to your application:

   For .NET applications:
   ```yaml
   annotations:
     instrumentation.opentelemetry.io/inject-dotnet: "splunk-otel-collector/splunk-otel-collector"
     instrumentation.opentelemetry.io/otel-dotnet-auto-runtime: "linux-x64"
   ```

   For NodeJS applications:
   ```yaml
   annotations:
     instrumentation.opentelemetry.io/inject-nodejs: "splunk-otel-collector/splunk-otel-collector"
   ```

   For Java applications:
   ```yaml
   annotations:
     instrumentation.opentelemetry.io/inject-java: "splunk-otel-collector/splunk-otel-collector"
   ```

   For Python applications:
   ```yaml
   annotations:
     instrumentation.opentelemetry.io/inject-python: "splunk-otel-collector/splunk-otel-collector"
   ```

## 📊 Splunk OpenTelemetry Integration

The Splunk OpenTelemetry Collector is a key component in this GitOps architecture, providing comprehensive observability for all applications.

### Features

- **Automatic Log Collection**: Captures container logs from all pods
- **Kubernetes Metrics**: Collects metrics from Kubelet API
- **Application Instrumentation**: Auto-instruments applications using agents
- **Span Collection**: Captures distributed traces for request flows
- **Direct Splunk Integration**: Forwards all telemetry to Splunk Cloud

### Configuration Components

The collector configuration is divided into several parts:

1. **Receivers**: Define data input sources
    - `filelog`: Container log files
    - `kubeletstats`: Kubernetes metrics
    - `otlp`: OpenTelemetry Protocol for traces and metrics

2. **Processors**: Transform and enrich data
    - `batch`: Group data for efficient transmission
    - `k8sattributes`: Add Kubernetes metadata
    - `resourcedetection`: Detect cloud provider information

3. **Exporters**: Define data output destinations
    - `splunk_hec`: Send metrics to Splunk
    - `splunk_hec/logs`: Send logs to Splunk

### Verification

After deployment, verify the collector is working correctly:

```bash
# Check collector pods
kubectl get pods -n splunk-otel-collector

# Check collector logs
kubectl logs -n splunk-otel-collector -l app.kubernetes.io/name=splunk-otel-collector -f

# Verify applications are instrumented
kubectl get pods -n eks-dev -o jsonpath='{.items[*].metadata.annotations}' | grep instrumentation
```

## 🔀 GitOps Workflow in Action

The GitOps workflow in this architecture follows these steps:

1. **Development**
    - Developer makes code changes to application
    - CI pipeline builds and pushes container image
    - Developer updates image tag in values file
    - Changes are committed to Git repository

2. **Detection**
    - ArgoCD detects changes in Git repository
    - Changes are analyzed against current state

3. **Validation**
    - Pre-sync health checks run before applying changes
    - Current deployments are validated for health

4. **Deployment**
    - ArgoCD applies changes to the cluster
    - Resources are created or updated following sync waves

5. **Verification**
    - Post-deployment health checks confirm successful rollout
    - Splunk begins collecting telemetry from updated applications

### Sync Waves and Ordered Deployment

Applications are deployed in specific order using sync waves:

1. **Infrastructure (Wave 0)**: splunk-otel-collector is deployed first
2. **Backend Services (Wave 1)**: API services are deployed next
3. **Frontend Applications (Wave 2)**: UI applications are deployed last

This ensures dependencies are available before dependent applications are deployed.

## 🔍 Troubleshooting

Common issues and solutions:

### ArgoCD Sync Issues

- **Problem**: Application shows "OutOfSync" but doesn't sync
    - **Solution**: Check Application's sync status in ArgoCD UI
    - **Command**: `argocd app get <app-name> --refresh`

- **Problem**: Sync fails with error
    - **Solution**: Check the error in ArgoCD UI and logs
    - **Command**: `kubectl logs -n argocd deployment/argocd-application-controller`

### Helm Chart Problems

- **Problem**: Helm template rendering errors
    - **Solution**: Validate templates locally
    - **Command**: `helm template HelmCharts/your-app --debug`

- **Problem**: Invalid chart structure
    - **Solution**: Verify chart structure follows Helm best practices
    - **Reference**: [Helm Best Practices](https://helm.sh/docs/chart_best_practices/)

### Splunk Collector Issues

- **Problem**: No data in Splunk
    - **Check**: Verify HEC token and endpoint configuration
    - **Command**: `kubectl get secret -n splunk-otel-collector splunk-otel-collector-secrets -o yaml`

- **Problem**: Collector pods crashing
    - **Check**: Inspect collector logs
    - **Command**: `kubectl logs -n splunk-otel-collector -l app.kubernetes.io/name=splunk-otel-collector`

## 🚀 Advanced Topics

### Multi-Cluster Management

This architecture can be extended to manage multiple clusters:

1. **Create additional environment directories** (e.g., `eks-staging`)
2. **Configure ArgoCD with multiple clusters**
   ```bash
   argocd cluster add <cluster-name>
   ```
3. **Adjust application manifests** to target specific clusters
   ```yaml
   spec:
     destination:
       name: cluster-name
       namespace: target-namespace
   ```

### CI/CD Integration

Integrate the GitOps flow with CI/CD pipelines:

1. **CI Pipeline** - Build and test application code
    - Triggered by code commits
    - Builds container images
    - Runs tests and security scans
    - Pushes images to registry

2. **CD Integration** - Update image versions in Git
    - Updates image tags in values files
    - Commits changes to trigger ArgoCD sync
    - Optionally use tools like Kustomize for dynamic replacements

### Disaster Recovery

Implement disaster recovery for the entire architecture:

1. **Backup ArgoCD State**
   ```bash
   argocd admin export > argocd-backup.yaml
   ```

2. **Infrastructure Recovery Procedure**
    - Store procedure in documentation
    - Include steps to recover EKS cluster
    - Include steps to reinstall ArgoCD
    - Include steps to apply root application

3. **Multi-region Redundancy**
    - Configure additional clusters in different regions
    - Use global DNS for failover
    - Replicate data between regions

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Contribution Guidelines

- Follow [Helm best practices](https://helm.sh/docs/chart_best_practices/) for Helm charts
- Use [Conventional Commits](https://www.conventionalcommits.org/) for commit messages
- Include meaningful comments in complex templates
- Update documentation when adding features

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.
