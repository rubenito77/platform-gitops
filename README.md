🚀 Platform GitOps Lab









🧠 Overview

Plataforma DevOps basada en GitOps + Observabilidad, desplegada sobre Kubernetes (K3s).

Incluye:

⚙️ Infraestructura automatizada (Ansible)<br>
🔁 GitOps con ArgoCD<br>
📦 Registry privado (Harbor)<br>
🌐 Ingress con Traefik + TLS<br>
📊 Observabilidad completa (Prometheus + Grafana)<br>
☸️ Métricas de Kubernetes (kube-state-metrics)<br>
<br>
🏗️ Arquitectura
![Project Structure](docs/images/arquitectura.png)
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
🌐 Accesos<br>
Servicio	URL<br>
Nginx	https://nginx.platform.local:31788<br>
Grafana	https://grafana.platform.local:31788<br>
Prometheus	https://prometheus.platform.local:31788<br>
📊 Observabilidad<br>
🔹 Prometheus<br>

Recolecta métricas de:<br>

Kubernetes<br> 
kube-state-metrics<br>
🔹 kube-state-metrics<br>

Expone métricas como:<br>

kube_pod_info<br>
kube_deployment_status_replicas<br>
kube_node_info<br>
<br>
🔹 Grafana<br>
Datasource: Prometheus<br>
Dashboards gestionados por GitOps<br>
Provisioning automático vía ConfigMap<br>
<br>
📊 Dashboards<br>

Carga automática desde:<br>

/var/lib/grafana/dashboards<br>

Provider:<br>

/etc/grafana/provisioning/dashboards<br>

⚠️ Requiere restart del pod para aplicar cambios<br>

🔐 Seguridad<br>
TLS self-signed<br>
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout tls.key \
  -out tls.crt \
  -subj "/CN=*.platform.local/O=platform-lab"<br>
<br>
Harbor (HTTP)<br>
docker login 192.168.1.24<br>
<br>
🔍 Validaciones<br>
kubectl get pods -A<br>
kubectl get svc -A<br>
kubectl get ingress -A<br>
kubectl get applications -n argocd<br>
<br>
🧪 Testing<br>
curl -k https://nginx.platform.local:31788<br>
curl -k https://grafana.platform.local:31788<br>
curl -k https://prometheus.platform.local:31788<br>
<br>
🧠 Networking<br>
Internal: 192.168.58.x<br>
External: 192.168.1.x<br>
<br>
💡 Best Practices<br>
Git como fuente de verdad<br>
No versionar secretos<br>
Versionar imágenes (no latest)<br>
Namespaces por app<br>
Todo vía GitOps<br>
<br>
📌 Roadmap<br>
Node Exporter (infra metrics)<br>
Kubernetes Dashboard PRO<br>
Alertmanager<br>
Keycloak (SSO)<br>
cert-manager (TLS real)<br>
Vault / External Secrets<br>
Multi-env (dev / uat / prod)<br>
DNS real<br>
<br>
👨‍💻 Autor<br>

Rubinho<br>
DevOps • Kubernetes • Middleware<br>
