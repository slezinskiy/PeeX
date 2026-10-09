# DevOps - Containers

[⬅ Назад до README](../README.md)

| # | Питання | Key | Інфраструктура |
|---|---------|-----|----------------|
| CNT-1 | [Implement and manage autoscaling for containerized workloads](#cnt-1-implement-and-manage-autoscaling-for-containerized-workloads) | | 📘 NEBo Task |
| CNT-2 | [Implement and manage secret management for containerized workloads](#cnt-2-implement-and-manage-secret-management-for-containerized-workloads) | ✅ | 📘 NEBo Task (+ 🤖 доповнення Claude) |
| CNT-3 | [Optimize and secure service-to-service network traffic in containerized environments](#cnt-3-optimize-and-secure-service-to-service-network-traffic-in-containerized-environments) | | 📘 NEBo Task |

> **Легенда:** 📘 NEBo Task — офіційний таск із вихідного файлу; 🤖 Згенеровано Claude — таск створено Claude за зразком NEBo Tasks, це не офіційний матеріал.

---

## CNT-1. Implement and manage autoscaling for containerized workloads

**Description**

Design, configure, and maintain autoscaling mechanisms for containerized environments. This includes implementing node autoscaling, configuring container resource limits, and enabling container autoscaling based on documented application requirements and workload demand. The solution should adapt dynamically to traffic fluctuations while optimizing resource utilization across different container platforms.

**Outcomes**

- Manifest file including the autoscaling options based on the documented application requirements analysis
- Kubernetes cluster autoscaler configuration based on the requirements of the application. This should also include containers' resource limits configuration.

### 🏗️ Інфраструктура — 📘 NEBo Task: DevOps - Implement and manage autoscaling for containerized workloads

**Task Description**

This task covers designing, configuring, and maintaining autoscaling mechanisms for containerized environments. This includes implementing Horizontal Pod Autoscaler (HPA), Vertical Pod Autoscaler (VPA) if applicable, and cluster autoscaler for node scaling. The work involves configuring container resource requests and limits, enabling autoscaling based on CPU, memory, or custom metrics, and ensuring that the system adapts dynamically to workload demand while optimizing resource usage. The solution must demonstrate both pod-level and node-level scaling behaviors. This task confirms the ability to implement production-grade autoscaling for containerized workloads.

**Prerequisites**

- Kubernetes cluster (cloud-managed preferred for cluster autoscaler)
- Metrics Server installed
- kubectl configured
- Understanding of Kubernetes resources and HPA concepts
- Sample application that can generate load
- Monitoring tools (Prometheus/Grafana recommended)

**Guidance and Tips**

- Environment: Use cloud-managed Kubernetes (EKS, AKS, GKE) for full cluster autoscaler support
- Time Planning: Plan around 12-16 hours for autoscaler configuration, testing, and monitoring setup
- Scope: Implement HPA for pods, cluster autoscaler for nodes, and demonstrate scaling under load
- Grade C Tips: Start with CPU-based HPA, progress to custom metrics. Test both scale-out and scale-in. Monitor scaling behavior. Document resource requirements analysis. Set appropriate limits to prevent runaway scaling

**Acceptance Criteria**

Functional Requirements

- A Horizontal Pod Autoscaler is configured using appropriate metrics (for example, CPU, memory, or custom metrics).
- HPA scale-out behavior is demonstrated under load (pods increase when thresholds are exceeded).
- HPA scale-in behavior is demonstrated when load decreases (pods decrease once thresholds return to normal).
- A cluster autoscaler is configured and demonstrates:
  - Node additions when pods are pending due to insufficient capacity.
  - Node removals when nodes are underutilized and workloads can be rescheduled.
- Resource requests and limits are properly configured to ensure accurate autoscaling decisions.
- Scaling behavior is monitored and visualized using dashboards or monitoring tools (for example, Prometheus/Grafana or cloud-native monitoring).

Non-Functional Requirements

- Scaling respects application requirements and SLAs, ensuring workloads remain available and performant during scaling events.
- Resource utilization is optimized through appropriate scaling thresholds and resource definitions.
- Scaling decisions are based on meaningful, well-calibrated metrics and avoid instability.
- Cooldown periods, stabilization windows, or equivalent mechanisms are applied to prevent thrashing.
- Cost optimization principles are followed, ensuring unnecessary scaling actions are avoided.
- Autoscaling configuration and observed behavior are documented clearly, including rationale for chosen thresholds.

**Outcome Artifacts**

- HPA configuration YAML showing metrics and thresholds
- Cluster autoscaler configuration
- Resource requests/limits in pod specifications
- Load testing script or tool configuration
- Monitoring graphs showing pod count changes during load test
- Monitoring graphs showing node count changes
- Screenshots of scaling events in cluster
- Documentation of scaling behavior and tuning decisions
- Short README documenting: autoscaling strategy, metrics used, testing procedures, and optimization recommendations

**Vendor Annex**

Kubernetes HPA Example
- Typical Tools: Kubernetes HPA, Metrics Server, kubectl
- References: HPA documentation, HPA walkthrough
- Key Commands:
  - `kubectl apply -f hpa.yaml`
  - `kubectl get hpa`
  - `kubectl top pods`

Custom Metrics HPA with Prometheus
- Typical Tools: Prometheus, Prometheus Adapter, Custom Metrics API
- References: Custom metrics, Prometheus Adapter
- Key Commands:
  - `kubectl apply -f custom-metrics-hpa.yaml`
  - `kubectl get --raw /apis/custom.metrics.k8s.io/`

AWS EKS Cluster Autoscaler
- Typical Tools: Cluster Autoscaler, AWS Auto Scaling Groups
- References: EKS Cluster Autoscaler
- Key Commands:
  - `kubectl apply -f cluster-autoscaler-autodiscover.yaml`
  - `kubectl -n kube-system edit deployment cluster-autoscaler`

Azure AKS Cluster Autoscaler
- Typical Tools: AKS Cluster Autoscaler
- References: AKS Cluster Autoscaler
- Key Commands:
  - `az aks update --resource-group <rg> --name <cluster> --enable-cluster-autoscaler --min-count 1 --max-count 10`

GCP GKE Cluster Autoscaler
- Typical Tools: GKE Cluster Autoscaler, Node Auto-provisioning
- References: GKE Cluster Autoscaler
- Key Commands:
  - `gcloud container clusters create <cluster> --enable-autoscaling --min-nodes 1 --max-nodes 10`
  - `gcloud container clusters update <cluster> --enable-autoprovisioning`

---

## CNT-2. Implement and manage secret management for containerized workloads

`Key`

**Description**

Design, configure, and maintain secret management solutions for containerized environments to ensure secure handling of sensitive data such as credentials, tokens, and keys.

**Outcomes**

- Documented and configured secrets manager integration for containerized environment.

### 🏗️ Інфраструктура — 📘 NEBo Task: DevOps - Implement and manage secret management for containerized workloads

**Task Description**

Implement and manage a secure secret management solution for containerized workloads. The goal is to externalize sensitive configuration data, such as credentials, API keys, and tokens from container images and manifests, ensuring they are securely stored, accessed, and rotated. This task focuses on integrating a secret management system with a container orchestration platform using a CSI (Container Storage Interface) driver or equivalent method, and validating secure runtime injection of secrets into applications.

**Acceptance Criteria**

Functional Requirements:

- A container orchestration environment (e.g., Kubernetes or equivalent) is deployed and operational.
- A secret management system is installed and configured (e.g., a cloud-managed secrets service or a vault-based solution).
- A secrets integration driver or plugin (e.g., CSI Secrets Store driver) is deployed and linked to the orchestration cluster.
- Secrets are created and securely stored in the secret management system, including:
  - Application credentials (e.g., database username/password).
  - API keys or tokens.
- Workload manifests (e.g., deployment YAML files or Helm charts) are updated to retrieve secrets securely at runtime, without hard-coding sensitive values.
- Secrets are mounted or injected into containers as environment variables or volumes.
- Access permissions for retrieving secrets are defined using least-privilege principles.

Non-Functional Requirements:

- Secrets are encrypted at rest and in transit using strong encryption mechanisms.
- Access control follows role-based authorization and enforces audit logging for secret retrieval actions.
- Secret rotation processes are automated or easily repeatable, ensuring minimal manual handling.
- Secret management configuration avoids exposure of sensitive information in build logs, manifests, or version control repositories.
- A cleanup procedure is included to remove test secrets and revoke associated access roles after verification.

#### 🤖 Доповнення — згенеровано Claude (у вихідному NEBo Task відсутні Prerequisites, Outcome Artifacts, Vendor Annex)

**Prerequisites**

- Kubernetes cluster (EKS / AKS / GKE / kind for Vault variant)
- Secrets backend: AWS Secrets Manager / Azure Key Vault / GCP Secret Manager / HashiCorp Vault
- Helm, kubectl
- Workload identity mechanism: IRSA / EKS Pod Identity / Azure Workload Identity / GKE Workload Identity / Vault Kubernetes auth

**Outcome Artifacts**

- Helm values / manifests for Secrets Store CSI Driver + provider (or External Secrets Operator / Vault Agent Injector)
- `SecretProviderClass` (or `ExternalSecret`) definitions
- IAM policy / Vault policy granting least-privilege access to specific secrets only
- Updated Deployment manifest showing volume mount / env from secret, without plaintext values
- Evidence of runtime injection (`kubectl exec ... -- ls /mnt/secrets-store`, app connecting to DB)
- Audit log entries of secret retrieval (CloudTrail `GetSecretValue`, Vault audit log)
- Rotation demonstration (secret rotated → pod receives new value via rotation poll / reloader)
- Cleanup script and output
- Short README: architecture, access model, rotation and cleanup procedures

**Vendor Annex**

- AWS: Secrets Manager (+ rotation Lambda), Secrets Store CSI Driver + AWS provider (ASCP), IRSA / Pod Identity, KMS
- Azure: Key Vault + Secrets Store CSI Driver provider, Workload Identity
- GCP: Secret Manager + CSI provider, Workload Identity
- HashiCorp Vault: Kubernetes auth method, Vault CSI provider / Agent Injector, dynamic DB credentials
- Alternative: External Secrets Operator, Sealed Secrets / SOPS (for GitOps)
- Key Commands: `helm install csi-secrets-store secrets-store-csi-driver/secrets-store-csi-driver --set syncSecret.enabled=true --set enableSecretRotation=true -n kube-system`, `aws secretsmanager rotate-secret --secret-id ...`

---

## CNT-3. Optimize and secure service-to-service network traffic in containerized environments

**Description**

Design, configure, and maintain networking solutions to enable secure, efficient, and observable communication between services in containerized environments. This includes implementing container networking (e.g., Docker Compose networking, Kubernetes network plugins, or service meshes), applying traffic management policies, securing connectivity through encryption and access controls, and enabling observability of network flows.

**Outcomes**

- Secured, and monitored network traffic between services in a cluster. Configured Load Balancers to secure and connect traffic between containerized services. Created a policy on a service that rate-limit all traffic to a version of a service from a specific origin.

### 🏗️ Інфраструктура — 📘 NEBo Task: DevOps - Optimize and secure service-to-service network traffic in containerized environments

**Task Description**

This task covers designing, configuring, and maintaining networking solutions that enable secure, efficient, and observable communication between services in containerized environments. The work includes implementing container networking components (for example, Kubernetes CNI plugins or service meshes such as Istio or Linkerd), applying traffic management policies, enforcing mTLS for encrypted communication, restricting connectivity through network policies, and enabling observability of service-to-service traffic. This task confirms the ability to implement production-grade service mesh, traffic control, and network security for microservices.

**Prerequisites**

- Kubernetes cluster
- Understanding of microservices networking and service mesh concepts
- kubectl and istioctl/linkerd CLI tools (if using service mesh)
- Sample multi-service application
- Understanding of network policies and security concepts
- Monitoring tools for traffic visualization

**Guidance and Tips**

- Environment: Use Kubernetes cluster with Istio or Linkerd service mesh
- Time Planning: Plan around 16-20 hours for service mesh setup, policy configuration, security implementation, and testing
- Scope: Implement service mesh, configure traffic management, enable mTLS, create network policies, and demonstrate traffic control
- Grade C Tips: Start with simple service mesh installation. Enable mTLS incrementally. Test traffic policies thoroughly. Monitor service-to-service communication. Document security architecture comprehensively

**Acceptance Criteria**

Functional Requirements

- A service mesh is deployed and configured (for example, Istio, Linkerd, or equivalent).
- Secure service-to-service communication is enabled using mTLS.
- Traffic management policies are implemented, including at least:
  - Retry logic
  - Request timeouts
  - Circuit breaker or outlier detection
- Rate limiting is configured for at least one service.
- Network policies are implemented to restrict pod-to-pod communication, allowing only necessary flows.
- Service-to-service traffic is observable through metrics or traces (for example, mesh telemetry, distributed tracing).
- Load balancing is configured across service versions or pods.

Non-Functional Requirements

- A zero-trust networking model is applied, ensuring all service-to-service communication is explicitly authorized.
- All inter-service traffic is encrypted in transit via mTLS.
- Traffic management and security policies do not introduce significant latency or instability.
- Policies are maintainable, well-structured, and documented, including their purpose and expected behavior.
- Observability provides actionable insights into traffic behavior, latency, error rates, and policy enforcement.

**Outcome Artifacts**

- Service mesh deployment configuration
- mTLS configuration showing encryption enabled
- Traffic management policy definitions (VirtualService, DestinationRule)
- Rate limiting policy with thresholds
- Network policy configurations
- Service mesh dashboard showing traffic flows
- Screenshots demonstrating policy enforcement (rate limiting triggered)
- Metrics showing encrypted traffic percentage
- Short README documenting: network architecture, security model, traffic policies, and operational procedures

**Vendor Annex**

Istio Service Mesh Example
- Typical Tools: Istio, kubectl, istioctl, Kiali, Jaeger
- References: Istio documentation, Istio traffic management, Istio security
- Key Commands:
  - `istioctl install --set profile=demo`
  - `kubectl label namespace default istio-injection=enabled`
  - `kubectl apply -f virtualservice.yaml`

Linkerd Service Mesh Example
- Typical Tools: Linkerd, linkerd CLI, Linkerd Viz
- References: Linkerd documentation, Getting started
- Key Commands:
  - `linkerd install | kubectl apply -f -`
  - `linkerd viz dashboard`
  - `kubectl get pods -o yaml | linkerd inject -`

Kubernetes Network Policies
- Typical Tools: Kubernetes Network Policy, Calico, Cilium
- References: Network Policies
- Key Commands:
  - `kubectl apply -f deny-all.yaml`
  - `kubectl apply -f allow-frontend-backend.yaml`
  - `kubectl get networkpolicies`
