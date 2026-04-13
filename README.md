🚀 Platform GitOps Lab









🧠 Overview

Plataforma DevOps basada en GitOps + Observabilidad, desplegada sobre Kubernetes (K3s).

Incluye:

⚙️ Infraestructura automatizada (Ansible)
🔁 GitOps con ArgoCD
📦 Registry privado (Harbor)
🌐 Ingress con Traefik + TLS
📊 Observabilidad completa (Prometheus + Grafana)
☸️ Métricas de Kubernetes (kube-state-metrics)
🏗️ Arquitectura
flowchart TD

A[GitHub Repo] --> B[ArgoCD]
B --> C[Kubernetes K3s]

C --> D[Traefik Ingress]

D --> E[Nginx App]
D --> F[Grafana]
D --> G[Prometheus]

G --> H[kube-state-metrics]
F --> G

C --> I[Harbor Registry]

## 📁 Estructura del proyecto

![Project Structure](docs/images/structure.png)


⚙️ GitOps Workflow
Git push → ArgoCD detecta → Sync automático → Kubernetes aplica

✔ Declarativo
✔ Automatizado
✔ Sin intervención manual

📦 Aplicación de ejemplo
nginx-harbor
image: 192.168.1.24/platform/nginx:v1

Incluye:

Deployment
Service (ClusterIP)
Ingress (Traefik)
TLS self-signed
🌐 Accesos
Servicio	URL
Nginx	https://nginx.platform.local:31788
Grafana	https://grafana.platform.local:31788
Prometheus	https://prometheus.platform.local:31788
📊 Observabilidad
🔹 Prometheus

Recolecta métricas de:

Kubernetes
kube-state-metrics
🔹 kube-state-metrics

Expone métricas como:

kube_pod_info
kube_deployment_status_replicas
kube_node_info
🔹 Grafana
Datasource: Prometheus
Dashboards gestionados por GitOps
Provisioning automático vía ConfigMap
📊 Dashboards

Carga automática desde:

/var/lib/grafana/dashboards

Provider:

/etc/grafana/provisioning/dashboards

⚠️ Requiere restart del pod para aplicar cambios

🔐 Seguridad
TLS self-signed
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout tls.key \
  -out tls.crt \
  -subj "/CN=*.platform.local/O=platform-lab"
Harbor (HTTP)
docker login 192.168.1.24
🔍 Validaciones
kubectl get pods -A
kubectl get svc -A
kubectl get ingress -A
kubectl get applications -n argocd
🧪 Testing
curl -k https://nginx.platform.local:31788
curl -k https://grafana.platform.local:31788
curl -k https://prometheus.platform.local:31788
🧠 Networking
Internal: 192.168.58.x
External: 192.168.1.x
💡 Best Practices
Git como fuente de verdad
No versionar secretos
Versionar imágenes (no latest)
Namespaces por app
Todo vía GitOps
📌 Roadmap
Node Exporter (infra metrics)
Kubernetes Dashboard PRO
Alertmanager
Keycloak (SSO)
cert-manager (TLS real)
Vault / External Secrets
Multi-env (dev / uat / prod)
DNS real
👨‍💻 Autor

Ruben Macchi
DevOps • Kubernetes • Middleware
