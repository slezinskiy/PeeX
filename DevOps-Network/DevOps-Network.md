# DevOps - Network

[⬅ Назад до README](../README.md)

| # | Питання | Key | Інфраструктура |
|---|---------|-----|----------------|
| NET-1 | [Implement and manage load balancing solutions](#net-1-implement-and-manage-load-balancing-solutions) | ✅ | 📘 NEBo Task |

> **Легенда:** 📘 NEBo Task — офіційний таск із вихідного файлу; 🤖 Згенеровано Claude — таск створено Claude за зразком NEBo Tasks, це не офіційний матеріал.

---

## NET-1. Implement and manage load balancing solutions

`Key`

**Description**

Configure and manage load balancers to distribute client requests across multiple application servers using predefined routing algorithms. Apply appropriate load balancing types (e.g., application, network, global, or gateway).

**Outcomes**

- Created and configured load balancing solutions with efficient request distribution and support for features such as SSL offloading and traffic routing.

### 🏗️ Інфраструктура — 📘 NEBo Task: DevOps - Implement and manage load balancing solutions

**Task Description**

This task covers configuring and managing load balancing solutions that distribute client requests across multiple application servers using appropriate routing algorithms and health checks. The work includes deploying application load balancers, network load balancers, or global load balancers; configuring SSL/TLS termination; implementing routing rules such as path-based or host-based routing; and validating automatic failover and request distribution. The solution must demonstrate an operational, production-grade load balancing setup capable of handling failures, scaling demands, and enforcing secure communication.

**Prerequisites**

- Access to cloud provider account with load balancing services
- Multiple application instances or containers for backend
- Understanding of OSI model (Layer 4 vs Layer 7)
- SSL/TLS certificates (for HTTPS)
- DNS domain for testing (optional)
- Understanding of load balancing algorithms

**Guidance and Tips**

- Environment: Use cloud provider load balancing services (AWS ALB/NLB, Azure Load Balancer, GCP Load Balancer)
- Time Planning: Plan around 8-12 hours for load balancer setup, backend configuration, health checks, and testing
- Scope: Implement one load balancer type with multiple backends, health checks, and traffic routing
- Grade C Tips: Understand when to use Layer 4 vs Layer 7. Configure health checks properly. Test failover scenarios. Implement SSL termination. Monitor load distribution. Document routing decisions

**Acceptance Criteria**

Functional Requirements

- A load balancer is created and configured using the appropriate type for the application workload.
- Multiple backend instances, with a minimum of two, are registered with the load balancer.
- Health checks are configured and actively monitor backend instance health.
- Traffic is distributed across all healthy backend instances according to the routing configuration.
- Backend instances that fail health checks are automatically removed from the active rotation.
- SSL or TLS termination is configured when using an application-layer load balancer.
- Routing rules such as path-based routing or host-based routing are implemented and validated.
- Connectivity tests confirm that requests are routed correctly under normal conditions and during backend failures.
- Monitoring and logging are enabled to observe load balancer performance and traffic behavior.

Non-Functional Requirements

- Traffic distribution remains balanced according to the selected routing algorithm.
- Health checks detect backend failures quickly to minimize disruption.
- SSL or TLS certificates used by the load balancer are properly configured, valid, and secure.
- The load balancer handles traffic spikes or increased request volume without service degradation.
- Configuration follows security best practices, including limiting exposure of management interfaces and enforcing encrypted communication.
- Documentation clearly describes load balancer architecture, routing rules, health check logic, and operational considerations.
- Routing decisions remain predictable and consistent across deployments.
- Changes to configuration are maintainable and traceable through version control.

**Outcome Artifacts**

- Load balancer configuration showing type and settings
- Backend target group/pool configuration
- Health check configuration and status
- SSL certificate configuration (if applicable)
- Routing rules configuration
- Monitoring showing traffic distribution across backends
- Test results showing failover when backend becomes unhealthy
- Performance metrics or load testing results
- Short README documenting: architecture, routing logic, health check settings, and operational procedures

**Vendor Annex**

AWS Load Balancing (ALB/NLB)
- Typical Tools: AWS ALB, NLB, Target Groups, AWS CLI
- References: Application Load Balancer, Network Load Balancer
- Key Commands:
  - `aws elbv2 create-load-balancer ...`
  - `aws elbv2 create-target-group ...`
  - `aws elbv2 register-targets ...`
  - `aws elbv2 create-listener ...`

Azure Load Balancer & Application Gateway
- Typical Tools: Azure Load Balancer, Application Gateway, Azure CLI
- References: Azure Load Balancer, Azure Application Gateway
- Key Commands:
  - `az network lb create ...`
  - `az network lb probe create ...`
  - `az network lb rule create ...`
  - `az network application-gateway create ...`

GCP Load Balancing
- Typical Tools: GCP HTTP(S) Load Balancer, TCP/UDP Load Balancer, gcloud CLI
- References: GCP Load Balancing Overview
- Key Commands:
  - `gcloud compute instance-groups create ...`
  - `gcloud compute health-checks create ...`
  - `gcloud compute backend-services create ...`
  - `gcloud compute forwarding-rules create ...`

Non-Cloud / On-Prem Load Balancing
- Typical Tools: Nginx, HAProxy, Envoy
- References: Nginx Load Balancing Guide, HAProxy Documentation
- Key Commands:
  - `nginx -t && nginx -s reload`
  - `haproxy -f /etc/haproxy/haproxy.cfg -c`
  - `systemctl restart nginx`
