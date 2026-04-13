🚀 Platform GitOps Lab










🧠 Platform Engineering Lab (GitOps + Observability)

Este proyecto implementa una plataforma moderna de DevOps basada en principios de:

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
flowchart TD

Dev[Developer Push] --> Git[GitHub Repo]

Git --> Argo[ArgoCD]
Argo --> K8s[Kubernetes K3s Cluster]

K8s --> Ingress[Traefik Ingress]

Ingress --> App[Nginx App]
Ingress --> Grafana[Grafana UI]
Ingress --> Prometheus[Prometheus UI]

Prometheus --> Metrics[kube-state-metrics]
Grafana --> Prometheus

K8s --> Harbor[Harbor Registry]
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
├── ansible/                  # Provisioning de infraestructura
├── bootstrap/               # Bootstrapping (ArgoCD)
├── apps/
│   ├── nginx-harbor/        # Aplicación de ejemplo
│   └── monitoring/          # Observabilidad completa
├── argocd/                  # Applications GitOps
├── certs/                   # Certificados (no versionado)
└── README.md
🔁 Flujo GitOps
Code → Git Push → ArgoCD Sync → Kubernetes Apply → Runtime

✔ Declarativo
✔ Reproducible
✔ Automatizado

📦 Aplicaciones desplegadas
🔹 Nginx (demo app)
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

Kubernetes cluster
kube-state-metrics

Ejemplo:

kube_pod_info
kube_deployment_status_replicas
Grafana
Datasource: Prometheus
Dashboards gestionados por Git
Provisioning automático vía ConfigMaps
📊 Dashboards as Code
Grafana dashboards = versionados en Git

Ubicación:

apps/monitoring/grafana-dashboard-k8s.yaml

Provisioning:

/etc/grafana/provisioning/dashboards
🔐 Seguridad
TLS self-signed
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout tls.key \
  -out tls.crt \
  -subj "/CN=*.platform.local/O=platform-lab"
Harbor Registry
docker login 192.168.1.24
🚀 Quick Start (lab reproducible)
1. Provisioning
ansible-playbook -i ansible/hosts.ini ansible/install_base.yml
ansible-playbook -i ansible/hosts.ini ansible/install_k3s.yml
2. Bootstrap ArgoCD
kubectl apply -f bootstrap/argocd/argocd-nodeport.yaml
3. Deploy Apps (GitOps)
kubectl apply -f argocd/
🧪 Validaciones
kubectl get pods -A
kubectl get ingress -A
kubectl get applications -n argocd
📈 Resultados obtenidos
✔ GitOps funcionando end-to-end
✔ Aplicaciones desplegadas automáticamente
✔ Observabilidad integrada
✔ Dashboards versionados
✔ Infraestructura reproducible
💡 Buenas prácticas implementadas
Git como única fuente de verdad
Separación por namespaces
Configuración declarativa
Versionado de dashboards
Automatización completa
No uso de latest en imágenes
📌 Roadmap
Node Exporter (CPU/memoria real)
Alertmanager
Keycloak (SSO)
cert-manager (TLS real)
Vault / External Secrets
Multi-cluster
CI/CD pipeline (GitHub Actions)
🧠 Lecciones clave
- GitOps simplifica la operación
- Observabilidad es fundamental desde el inicio
- Todo debe ser declarativo
- Kubernetes + GitOps = plataforma escalable
👨‍💻 Autor

Ruben Macchi
DevOps • Platform Engineer • Kubernetes
