🚀 Platform GitOps Lab














🧠 Platform Engineering Lab (GitOps + Observability)

Este proyecto implementa una plataforma moderna de DevOps basada en:

GitOps
Infraestructura como código
Observabilidad
Automatización end-to-end

👉 Diseñado para simular un entorno real de producción.

🎯 Objetivos del proyecto
Implementar un flujo GitOps real con ArgoCD
Desplegar aplicaciones desde un registry privado (Harbor)
Exponer servicios vía Ingress + TLS
Implementar observabilidad completa (Prometheus + Grafana)
Versionar dashboards y configuración como código
Automatizar la infraestructura desde cero
🏗️ Arquitectura
⚙️ Stack tecnológico
Componente	Tecnología
Orquestación	K3s
GitOps	ArgoCD
Ingress	Traefik
Registry	Harbor (HTTP)
Monitoring	Prometheus
Visualization	Grafana
Metrics K8s	kube-state-metrics
IaC	Ansible
📁 Estructura del repositorio
platform/
├── ansible/
├── bootstrap/
├── apps/
│   ├── nginx-harbor/
│   └── monitoring/
├── argocd/
├── certs/
└── README.md
🔁 Flujo GitOps
Code → Git Push → ArgoCD Sync → Kubernetes Apply → Runtime

✔ Declarativo
✔ Reproducible
✔ Automatizado

📦 Aplicaciones desplegadas
🔹 Nginx
image: 192.168.1.24/platform/nginx:v1
🔹 Monitoring Stack
Prometheus
Grafana
kube-state-metrics
🌐 Accesos
Servicio	URL
Nginx	https://nginx.platform.local:31788

Grafana	https://grafana.platform.local:31788

Prometheus	https://prometheus.platform.local:31788
📊 Observabilidad
Prometheus

Recolecta métricas de:

Kubernetes
kube-state-metrics

Ejemplo:

kube_pod_info
kube_deployment_status_replicas
Grafana
Datasource: Prometheus
Dashboards gestionados por Git
Provisioning automático vía ConfigMaps
📊 Dashboards as Code

Los dashboards están versionados en Git:

apps/monitoring/grafana-dashboard-k8s.yaml

Se cargan automáticamente desde:

/var/lib/grafana/dashboards
🔐 Seguridad
TLS self-signed
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout tls.key \
  -out tls.crt \
  -subj "/CN=*.platform.local/O=platform-lab"
Harbor
docker login 192.168.1.24
🚀 Quick Start
1. Provisioning
ansible-playbook -i ansible/hosts.ini ansible/install_base.yml
ansible-playbook -i ansible/hosts.ini ansible/install_k3s.yml
2. Bootstrap ArgoCD
kubectl apply -f bootstrap/argocd/argocd-nodeport.yaml
3. Deploy Apps
kubectl apply -f argocd/
🧪 Validaciones
kubectl get pods -A
kubectl get ingress -A
kubectl get applications -n argocd
📈 Resultados
✔ GitOps funcionando end-to-end
✔ Observabilidad integrada
✔ Dashboards versionados
✔ Infraestructura reproducible
💡 Buenas prácticas
Git como única fuente de verdad
No subir secretos
Evitar latest
Namespaces por aplicación
Todo vía GitOps
📌 Roadmap
Node Exporter
Alertmanager
Keycloak (SSO)
cert-manager
Vault / External Secrets
Multi-entorno
👨‍💻 Autor

Ruben Macchi
DevOps • Platform Engineer • Kubernetes
